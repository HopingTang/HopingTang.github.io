---
layout: post
title: "Notes for MultiView Geometry in CV"
date: 2016-06-13 22:00:00 +0800
---

# Notes for Multiple View Geometry in Computer Vision

## 1. Introduction – a Tour of Multiple View Geometry

### 1.1 Introduction – the ubiquitous projective geometry

The points at infinity in the two-dimensional projective space form a line, 
usually called the **line at infinity**. In three-dimensions they form the **plane at infinity**.

<!-- excerpt -->

...**projective transformation**, in which 
the coordinate vector is multiplied by a non-singular matrix. 
Under such a mapping, points at infinity (with final coordinate zero) 
are mapped to arbitrary other points. The points at infinity are not preserved.

The geometry of the projective plane and a distinguished line 
is known as **affine geometry** and any projective transformation that 
maps the distinguished line in one space to the distinguished line of 
the other space is known as an **affine transformation**.

The equation for a circle in homogeneous coordinates $$ (x, y, w) $$ is of the form

$$ (x − aw)^2 + (y − bw)^2 = r^2 w^2 $$

Every circle passes through the points $$ (1, ±i, 0)^\intercal $$ , 
and therefore they lie in the intersection of any two circles. 
Since their final coordinate is zero, these two points lie on the line at infinity. 
For obvious reasons, they are called the **circular points** of the plane.

Euclidean geometry arises from projective geometry by singling out first a line at infinity 
and subsequently, two points **called circular** points lying on this line. 

This line of thought leads to the discovery that in homogeneous coordinates 
$$ (X, Y, Z, T)^\intercal $$ all spheres intersect the plane at infinity in a curve with the equations: 
$$ X^2 + Y^2 + Z^2 = 0 $$ ; $$ T = 0 $$ . This is a second-degree curve (a conic) lying on the plane at infinity, 
and consisting only of complex points. 
It is known as the **absolute conic** and is one of the key geometric entities in this book, 
most particularly because of its connection to **camera calibration**, as will be seen later.

