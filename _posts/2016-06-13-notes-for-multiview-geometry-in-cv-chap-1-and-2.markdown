---
layout: post
title: "Notes for MultiView Geometry in CV Chap 1 and 2"
date: 2016-06-13 22:00:00 +0800
---

# Notes for Chapter 1 & 2 of Multiple View Geometry in Computer Vision

- [Chapter 1. Introduction – the ubiquitous projective geometry](#Chap1)  
- [Chapter 2. Projective Geometry and Transformations of 2D](#Chap2)

<a name="Chap1"></a>## 1. Introduction – a Tour of Multiple View Geometry

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
acting on the homogeneous coordinates of the point in $$ \mathbb{P}^3 $$
mapping it to the imaged point in $$ \mathbb{P}^2 $$ .
This matrix P is known as the **camera matrix**.

$$ \begin{pmatrix} x \\ y \\ w \end{pmatrix} = \mathbf{P}_{3×4}  \begin{pmatrix} X \\ Y \\ Z \\ T \end{pmatrix} $$

Furthermore, if all the points lie on a plane
(we may choose this as the plane Z = 0) then the linear mapping reduces to 

$$ \begin{pmatrix} x \\ y \\ w \end{pmatrix} = \mathbf{H}_{3×3} \begin{pmatrix} X \\ Y \\ T \end{pmatrix} $$

which is a **projective transformation**.

The absolute conic, however being a conic in the plane at infinity
must project to a conic in the image.
The resulting image curve is called the **Image of the Absolute Conic**,
or IAC. If the location of the IAC is known in an image,
then we say that the camera is **calibrated**.

Knowing the IAC, one can measure the angle between rays by direct measurements in the image.

### 1.3 Reconstruction from more than one view

In the two-view case, therefore, we consider a set of correspondences
xi ↔ xi’ in two images. It is assumed that there exist some camera matrices,
$$ \mathbf{P} $$ and $$ \mathbf{P}^{\prime} $$ and a set of 3D points $$ X_i $$ that give rise to
these image correspondences in the sense that $$ \mathbf{P} X_i = x_i $$ and $$ \mathbf{P}^{\prime} X_i = x_i^{\prime} $$.
Thus, the point $$ X_i $$ projects to the two given data points.
However, neither the cameras (represented by projection matrices $$ \mathbf{P} $$ and $$ \mathbf{P}^{\prime} $$),
nor the points $$ X_i $$ are known. It is our task to determine them.

It is possible to apply a projective transformation (represented by a 4 × 4 matrix $$ \mathbf{H} $$)
to each point $$ X_i $$ , and on the right of each camera matrix $$ P_j $$ ,
without changing the projected image points, thus:

$$ P_j X_i = (P_j \mathbf{H}^{-1} ) (\mathbf{H} X_i) $$

There is no compelling reason to choose one set of points and camera matrices over the other.
The choice of \mathbf{H} is essentially arbitrary,
and we say that the reconstruction has a projective ambiguity,
or is a **projective reconstruction**.

It is possible to reconstruct a set of points from two views,
up to an unavoidable projective ambiguity. Well, to be able to say this,
we need to make a few qualifications; there must be suffi- ciently many points,
at least seven, and they must not lie in one of various well-defined **critical configurations**.

The basic tool in the reconstruction of point sets from two views is the **fundamental matrix**,
which represents the constraint obeyed by image points $$ x $$ and $$ x^{\prime} $$
if they are to be images of the same 3D point.
This constraint arises from the coplanarity of the camera centres of the two views,
the images points and the space point. Given the fundamental matrix $$ \mathbf{F} $$,
a pair of matching points $$ x_i ↔ x_i^{\prime} $$ must satisfy

$$ x_i^{\prime \intercal} \mathbf{F} x_i = 0 $$

where $$ \mathbf{F} $$ is a $$ 3 × 3 $$ matrix of rank 2.

Given the two cameras $$ (\mathbf{P}, \mathbf{P}^{\prime} ) $$ and the corresponding image point pairs $$ x_i ↔ x_i^{\prime} $$,
find the 3D point $$ X_i $$ that projects to the given image points.
Solving for $$ X $$ in this way is known as **triangulation**.

### 1.4 Three-view geometry

Whereas for two views, the basic algebraic entity is the fundamental matrix,
for three views this role is played by the **trifocal tensor**.
The trifocal tensor is a $$ 3 × 3 × 3 $$ array of numbers 
that relate the coordinates of corresponding points or lines in three views.
The trifocal tensor is of the form $$ \mathcal{T}_i ^{jk} $$ , having two upper and one lower index._

There is a point X in space that maps to x in the first image,
and to points $$ x^{\prime} $$ and $$ x'' $$ lying on the lines $$ l' $$ and $$ l'' $$ in the other two images.
The coordinates of these three images are then related via the trifocal tensor relationship:

$$ \displaystyle\sum_{ijk} x_i l_j^{\prime} l_k^{\prime \prime} \mathcal{T}_i ^{jk} = 0 $$

The 27 elements of the tensor are not independent,
however, but are related by a set of so called **internal constraints**._

The fundamental matrix (which is a 2-view tensor) also satisfies an internal constraint
but a relatively simple one: the elements obey $$ det \mathbf{F} = 0 $$.

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
known as the **affine camera**.
This camera model is a fair approximation to perspective projection
whenever the distance to the scene is large
compared with the difference in depth between the back and front of the scene.
If a set of points are visible in all of a set of n views involving an affine camera,
then a well-known algorithm, the **factorization algorithm**,
can be used to compute both the structure of the scene,
and the specific camera models in one step using the Singular Value Decomposition.

The dominant methodology for the general reconstruction problem is **bundle adjustment**.
This is an iterative method,
in which one attempts to fit a non-linear model to the measured data (the point correspondences).

Using n-view reconstruction techniques,
it is possible to carry out reconstructions automatically from quite long sequences of images.

### 1.6 Transfer

**Transfer**: given the position of a point in one (or more) image(s),
determine where it will appear in all other images of the set.

Suppose the camera rotates about its centre or that all the scene points of interest lie on a plane.
Then the appropriate multiple view relations
are the planar projective transformations between the images.
In this case, a point seen in just one image can be transferred to any other image.

### 1.7 Euclidean reconstruction

Now, suppose that we have computed a projective reconstruction of the world,
using calibrated cameras. By definition,
this means that the IAC is known in each of the images;
let it be denoted by ωi in the i-th image.
The back-projection of each ωi is a cone in space,
and the absolute conic must lie in the intersection of all the cones.
Two cones in general intersect in a fourth-degree curve,
but given that they must intersect in a conic, this curve must split into two conics.
Thus, reconstruction of the absolute conic from two images is not unique – rather,
there are two possible solutions in general. However, from three or more images,
the intersection of the cones is unique in general.
Thus the absolute conic is determined and with it the Euclidean structure of the scene.

Of course, if the Euclidean structure of the scene is known,
then so is the position of the absolute conic.
In this case we may project it back into each of the images,
producing the IAC in each image, and hence calibrating the cameras.
Thus **knowledge of the camera calibration is equivalent to
being able to determine the Euclidean structure of the scene**.

### 1.8 Auto-calibration

Generally given three cameras known to have the same calibration,
it is possible to determine the absolute conic, and hence the calibration of the cameras.

**Knowing the plane at infinity**.
Assuming one knows the plane at infinity,
one can back-project a hypothesised IAC from each of a sequence of images
and intersect the resulting cones with the plane at infinity.
This gives a linear constraint on the entries of the matrix representing the IAC.
Thus, auto-calibration is relatively simple,
once the plane at infinity has been identified.
The identification of the plane at infinity itself is substantially more difficult.

**Auto-calibration given square pixels in the image**.
If the cameras are partially calibrated,
then it is possible to complete the calibration starting from a projective reconstruction.
One interesting example is the square-pixel constraint on the cameras.
What this means is that a Euclidean coordinate system is known in each image.
In this case, the absolute conic,
lying in the plane at infinity in the world must meet the image plane in its two circular points.
The circular points in a plane are the two points where the absolute conic meets that plane.

<a name="Chap2"></a>## 2. Projective Geometry and Transformations of 2D

### 2.1 Planar geometry
A point is identified with a vector in terms of some coordinate basis.
A line is also identified with a vector,
and a conic section (more briefly, a conic) is represented by a symmetric matrix.

### 2.2 The 2D projective plane
**Homogeneous representation of lines.**
The vectors $$ (a, b, c)^\intercal $$ and $$ k(a, b, c)^\intercal $$ represent the same line,for any non-zero k.
An equivalence class of vectors under this equivalence relationship is known as a homogeneous vector.
Any particular vector $$ (a, b, c)^\intercal $$ is a representative of the equivalence class.
The set of equivalence classes of vectors in $$ \mathbb{R}^3 − (0, 0, 0)^\intercal $$ forms the projective space $$ \mathbb{P}^2 $$.
The notation $$ −(0, 0, 0)^\intercal $$ indicates that the vector $$ (0, 0, 0)^\intercal $$ ,
which does not correspond to any line, is excluded.

Homogeneous representation of points.
A point $$ \mathbf{x} = (x, y)^\intercal $$ lies on the line $$ \mathbf{l} = (a, b, c)^\intercal $$ if and only if $$ ax + by + c = 0 $$.

$$ (x, y, 1) (a, b, c)^\intercal = (x, y, 1) \mathbf{l} = 0 $$

The point x lies on the line l if and only if $$ \mathbf{x}^\intercal \mathbf{l} = 0 $$.
The set of vectors $$ (kx, ky, k)^\intercal $$ for varying values of $$ k $$
to be a representation of the point $$ (x, y)^\intercal $$ in \mathbb{R}^2.
Thus, just as with lines, points are represented by homogeneous vectors.

Degrees of freedom (dof).
To specify a point two values must be provided, namely its x- and y-coordinates.
A line is specified by two parameters (the two independent ratios $$ {a : b : c} $$) and so has two degrees of freedom.

Intersection of lines.
From the triple scalar product identity
$$ \mathbf{l} (\mathbf{l} × \mathbf{l}^{\prime} ) = \mathbf{l}^{\prime} (\mathbf{l} × \mathbf{l}^{\prime} ) = 0 $$,
we see that $$ \mathbf{l}^\intercal \mathbf{x} = \mathbf{l}^{\prime \intercal} \mathbf{x} = 0 $$.
Thus, the intersection of two lines l and l is the point $$ \mathbf{x} = \mathbf{l} × \mathbf{l}^{\prime} $$.

Consider the simple problem of determining the intersection of the lines $$ x = 1  $$ and $$ y = 1 $$.
$$  \mathbf{l} = (−1, 0, 1)^\intercal,    \mathbf{l}'  = (0, −1, 1)^\intercal $$.


