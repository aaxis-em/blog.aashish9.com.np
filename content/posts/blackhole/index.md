---
title: "Raytracing a black Hole."
date: 2026-01-17
draft: false
---

## Gallery

{{< carousel images="bh/*.png" aspectRatio="21-9" interval="1000" >}}

## Introduction

A black hole is an astronomical body so compact that its gravity prevents anything, including light, from escaping.

A Schwarzschild black hole is a theoretical black hole that is spherically symmetric, non-rotating, and uncharged, described by the Schwarzschild solution to Einstein’s field equations.And this is what we are trying to make atleast trying :/.

## Motivation

There’s a part of me that wants to be a physicist. I romanticize astronomy: stars, planets, comets, black holes.
The universe is far stranger and more beautiful.

Six months ago I ran into General Relativity, and it finally gave structure to ideas that used to feel abstract. Time dilation, length contraction suddenly they weren’t quirks, but consequences. Time wasn’t special or separate; it was just another dimension, fused with space into spacetime.

This raytracer won’t fully follow General Relativity we’re taking shortcuts to keep it manageable as a mini project. The focus is on creating something that looks right, rather than simulating every detail of curved spacetime. It’s computer graphics first, physics second.

## Methodology

![flowchart](/images/GravitationalLensing.png)

This is general Mecahnism of how it works.

For a Newtonian particle of mass m in a central potential F(r), the motion in its orbital plane satisfies
{{< katex >}}

$$
\frac{d^2 \vec{x}}{dt^2} = \frac{1}{m} \vec{F}(r) \\
$$

Where F is given by

$$
\vec{F}(r) = - \frac{3 h^2}{2r^5} \hat{r}
$$

where h is a constant representing angular momentum per unit mass, and r̂ is the radial unit vector. Integration of this system produces a trajectory x⃗(T), where T is an abstract time parameter distinct from Schwarzschild coordinate time t and proper time (which does not exist for photons). This trajectory parametrizes the lightlike geodesic.

### WorkFlow

The renderer operates on a per-pixel basis. For each pixel:

- A primary ray is initialized at the camera position with a direction calculated from the pixel coordinates.
- The ray is advanced iteratively using RK4 integration. At each step, the distance from the black hole is evaluated to determine if the ray has been captured, escaped, or continues propagating.

- When the ray intersects the accretion disk, the local emission intensity and color are computed and composited with previously accumulated contributions along the ray. Opacity decays with each interaction to prevent excessive energy accumulation.
- The integration continues until the ray meets a termination condition (capture, escape, or minimal opacity).
- The accumulated color is then tone-mapped, gamma-corrected, and written to the pixel in the output image.

### Code Snippets

Acceleartion or F/m we consisder mass 1 for light rays

```cpp
#ifndef SCHWARZSCHILD_H
#define SCHWARZSCHILD_H

#include "../raytracer/ray.h"
#include "../raytracer/vec3.h"
#include <cmath>

inline vec3 schwarzschild_acceleration(const ray &r) {
  double R = r.x.length();
  double factor = -1.5 * r.h * r.h / std::pow(R, 5);
  return r.x * factor;
}

#endif
```

RK4 ODE code specifically for the above mentioned equaiton

```cpp
#ifndef RK4_H
#define RK4_H

#include "../raytracer/ray.h"
#include "schwarzschild.h"

inline void rk4_step(ray &r, double dt) {

vec3 k1x = r.v;
vec3 k1v = schwarzschild_acceleration(r);

ray r2 = r;
r2.x += k1x _ (dt _ 0.5);
r2.v += k1v _ (dt _ 0.5);
vec3 k2x = r2.v;
vec3 k2v = schwarzschild_acceleration(r2);

ray r3 = r;
r3.x += k2x _ (dt _ 0.5);
r3.v += k2v _ (dt _ 0.5);
vec3 k3x = r3.v;
vec3 k3v = schwarzschild_acceleration(r3);

ray r4 = r;
r4.x += k3x _ dt;
r4.v += k3v _ dt;
vec3 k4x = r4.v;
vec3 k4v = schwarzschild_acceleration(r4);

r.x += (k1x + 2 _ k2x + 2 _ k3x + k4x) _ (dt / 6.0);
r.v += (k1v + 2 _ k2v + 2 _ k3v + k4v) _ (dt / 6.0);
}

#endif
```

Code for color accumulation we decide color based on postion which gives use temperature there and we determine color.

```cpp
#ifndef DISK_H
#define DISK_H

#include "../raytracer/vec3.h"
#include <algorithm>
#include <cmath>

inline vec3 temperature_to_rgb(double temp) {
  temp = std::max(1000.0, std::min(40000.0, temp));

  double t = temp / 100.0;
  double r, g, b;

  if (t <= 66.0) {
    r = 1.0;
  } else {
    r = t - 60.0;
    r = 1.292936186 * pow(r, -0.1332047592);
    r = std::max(0.0, std::min(1.0, r));
  }

  if (t <= 66.0) {
    g = t;
    g = 0.39008157876 * log(g) - 0.63184144378;
  } else {
    g = t - 60.0;
    g = 1.129890861 * pow(g, -0.0755148492);
  }
  g = std::max(0.0, std::min(1.0, g));

  if (t >= 66.0) {
    b = 1.0;
  } else if (t <= 19.0) {
    b = 0.0;
  } else {
    b = t - 10.0;
    b = 0.54320678911 * log(b) - 1.19625408914;
  }
  b = std::max(0.0, std::min(1.0, b));

  return vec3(r, g, b);
}

inline vec3 disk_color_and_intensity(const vec3 &p, double &intensity) {
  double r = std::sqrt(p.x() * p.x() + p.y() * p.y());
  double z = std::abs(p.z());
  if (z > 0.3) {
    intensity = 0.0;
    return vec3(0, 0, 0);
  }

  if (r < 3.0) {
    intensity = 0.0;
    return vec3(0, 0, 0);
  }

  if (r > 15.0) {
    intensity = 0.0;
    return vec3(0, 0, 0);
  }
  double temp_base = 8000.0; // Base temperature at r=3
  double temperature = temp_base * std::pow(3.0 / r, 0.75);

  double phi = std::atan2(p.y(), p.x());
  double spiral = std::sin(3.0 * phi + 2.0 * r);

  temperature *= (1.0 + 0.3 * spiral);

  double v_orbital = std::sqrt(1.0 / r); // Keplerian
  vec3 tangent(-p.y(), p.x(), 0);
  tangent = unit_vector(tangent);

  vec3 to_observer = unit_vector(vec3(1, 0, 0));
  double beta = dot(tangent * v_orbital, to_observer);

  double doppler = std::sqrt((1.0 - beta) / (1.0 + beta));
  temperature /= doppler;

  vec3 color = temperature_to_rgb(temperature);

  intensity = std::pow(temperature / temp_base, 4.0) * std::exp(-r / 8.0) *
              std::exp(-8.0 * z);

  intensity *= (1.0 + 0.5 * spiral);
  intensity = std::max(0.0, intensity);

  return color;
}

inline double disk_intensity(const vec3 &p) {
  double intensity;
  disk_color_and_intensity(p, intensity);
  return intensity;
}

#endif
```

[![Repo Link](https://img.shields.io/badge/GitHub-GravitationalLensing%20Project-blue?logo=github)](https://github.com/aaxis-em/GravitationalLensing)

See you till next time
