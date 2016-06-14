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
$$ (X, Y, Z, T)^\intercal $$ all spheres intersect the plane at infinity
in a curve with the equations: $$ X^2 + Y^2 + Z^2 = 0 $$ ; $$ T = 0 $$ .
This is a second-degree curve (a conic) lying on the plane at infinity, 
and consisting only of complex points. 
It is known as the **absolute conic** and is one of the key geometric entities in this book, 
most particularly because of its connection to **camera calibration**, as will be seen later.

### 1.2 Camera projections

The most general imaging projection is represented by an arbitrary 3×4 matrix of rank 3,
acting on the homogeneous coordinates of the point in IP3 mapping it to the imaged point in IP2.
This matrix P is known as the camera matrix.

 x y wT= P3×4   X Y Z T T  

Furthermore, if all the points lie on a plane
(we may choose this as the plane Z = 0) then the linear mapping reduces to 

x y wT= H3×3   X Y T T

which is a projective transformation.

The absolute conic, however being a conic in the plane at infinity
must project to a conic in the image.
The resulting image curve is called the Image of the Absolute Conic,
or IAC. If the location of the IAC is known in an image,
then we say that the camera is calibrated.

Knowing the IAC, one can measure the angle between rays by direct measurements in the image.

### 1.3 Reconstruction from more than one view

In the two-view case, therefore, we consider a set of correspondences 
xi ↔ xi’ in two images. It is assumed that there exist some camera matrices,
P and P and a set of 3D points Xi that give rise to
these image correspondences in the sense that PXi = xi and P Xi = x i.
Thus, the point Xi projects to the two given data points.
However, neither the cameras (represented by projection matrices P and P ),
nor the points Xi are known. It is our task to determine them. 

It is possible to apply a projective transformation (represented by a 4 × 4 matrix H)
to each point Xi, and on the right of each camera matrix Pj ,
without changing the projected image points, thus: 
PjXi = (PjH-1 ) (HXi).
There is no compelling reason to choose one set of points and camera matrices over the other.
The choice of H is essentially arbitrary,
and we say that the reconstruction has a projective ambiguity,
or is a projective reconstruction.

It is possible to reconstruct a set of points from two views,
up to an unavoidable projective ambiguity. Well, to be able to say this,
we need to make a few qualifications; there must be suffi- ciently many points,
at least seven, and they must not lie in one of various well-defined critical configurations.

The basic tool in the reconstruction of point sets from two views is the fundamental matrix,
which represents the constraint obeyed by image points x and x’
if they are to be images of the same 3D point.
This constraint arises from the coplanarity of the camera centres of the two views,
the images points and the space point. Given the fundamental matrix F,
a pair of matching points   xi ↔ xi’ must satisfy
x'iT F xi = 0
where F is a 3 × 3 matrix of rank 2. 

Given the two cameras (P, P’ ) and the corresponding image point pairs xi ↔ x’ i,
find the 3D point Xi that projects to the given image points.
Solving for X in this way is known as triangulation. 

### 1.4 Three-view geometry

Whereas for two views, the basic algebraic entity is the fundamental matrix,
for three views this role is played by the trifocal tensor.
The trifocal tensor is a 3 × 3 × 3 array of numbers 
that relate the coordinates of corresponding points or lines in three views.
The trifocal tensor is of the form Tijk , having two upper and one lower index.

There is a point X in space that maps to x in the first image,
and to points x’ and x’’ lying on the lines l’ and l’’ in the other two images.
The coordinates of these three images are then related via the trifocal tensor relationship:  
ijk xi lj' lk'' Tijk = 0

The 27 elements of the tensor are not independent,
however, but are related by a set of so called internal constraints.

The fundamental matrix (which is a 2-view tensor) also satisfies an internal constraint
but a relatively simple one: the elements obey detF=0.

Once the trifocal tensor is known,
it is possible to extract the three camera matrices from it,
and thereby obtain a reconstruction of the scene points and lines.
As ever, this reconstruction is unique only up to a 3D projective transformation;
it is a projective reconstruction.

### 1.5 Four view geometry and n-view reconstruction

Many methods have been considered for reconstruction from several views.
One way to proceed is to reconstruct the scene bit by bit,
using three-view or two-view techniques.

The task of reconstruction becomes easier if we are able to apply a simpler camera model,
known as the affine camera.
This camera model is a fair approximation to perspective projection
whenever the distance to the scene is large
compared with the difference in depth between the back and front of the scene.
If a set of points are visible in all of a set of n views involving an affine camera,
then a well-known algorithm, the factorization algorithm,
can be used to compute both the structure of the scene,
and the specific camera models in one step using the Singular Value Decomposition. 

The dominant methodology for the general reconstruction problem is bundle adjustment.
This is an iterative method,
in which one attempts to fit a non-linear model to the measured data (the point correspondences).

Using n-view reconstruction techniques,
it is possible to carry out reconstructions automatically from quite long sequences of images.

### 1.6 Transfer

Transfer: given the position of a point in one (or more) image(s),
determine where it will appear in all other images of the set.

Suppose the camera rotates about its centre or that all the scene points of interest lie on a plane.
Then the appropriate multiple view relations
are the planar projective transformations between the images.
In this case, a point seen in just one image can be transferred to any other image.

### 1.7 Euclidean reconstruction


