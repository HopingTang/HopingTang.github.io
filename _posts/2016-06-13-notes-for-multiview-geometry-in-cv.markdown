---
layout: post
title: "Notes for MultiView Geometry in CV"
date: 2016-06-13 22:00:00 +0800
---

# Notes for Chapter 1 & 2 of Multiple View Geometry in Computer Vision

- [Chapter 1. Introduction – the ubiquitous projective geometry](#Chap1)  
- [Chapter 2. Projective Geometry and Transformations of 2D](#Chap2)

<!-- excerpt -->

<a name="Chap1"></a>

## 1. Introduction – a Tour of Multiple View Geometry

### 1.1 Introduction – the ubiquitous projective geometry

The points at infinity in the two-dimensional projective space form a line,
usually called the **line at infinity**. In three-dimensions they form the **plane at infinity**.

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

The most general imaging projection is represented by an arbitrary $$ 3×4 $$ matrix of rank $$ 3 $$,
acting on the homogeneous coordinates of the point in $$ \mathbb{P}^3 $$
mapping it to the imaged point in $$ \mathbb{P}^2 $$ .
This matrix $$ \mathbf{P} $$ is known as the **camera matrix**.

$$ \begin{pmatrix} x \\ y \\ w \end{pmatrix} = \mathbf{P}_{3×4}  \begin{pmatrix} X \\ Y \\ Z \\ T \end{pmatrix} $$

Furthermore, if all the points lie on a plane
(we may choose this as the plane $$ Z = 0 $$) then the linear mapping reduces to 

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
$$ x_i ↔ x_i’ $$ in two images. It is assumed that there exist some camera matrices,
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

<a name="Chap2"></a>

## 2. Projective Geometry and Transformations of 2D

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

**The point x lies on the line l if and only if $$ \mathbf{x}^\intercal \mathbf{l} = 0 $$**.
The set of vectors $$ (kx, ky, k)^\intercal $$ for varying values of $$ k $$
to be a representation of the point $$ (x, y)^\intercal $$ in $$ \mathbb{R}^2 $$.
Thus, just as with lines, points are represented by homogeneous vectors.

**Degrees of freedom (dof).**
To specify a point two values must be provided, namely its x- and y-coordinates.
A line is specified by two parameters (the two independent ratios $$ {a : b : c} $$) and so has two degrees of freedom.

**Intersection of lines.**
From the triple scalar product identity
$$ \mathbf{l} (\mathbf{l} × \mathbf{l}^{\prime} ) = \mathbf{l}^{\prime} (\mathbf{l} × \mathbf{l}^{\prime} ) = 0 $$,
we see that $$ \mathbf{l}^\intercal \mathbf{x} = \mathbf{l}^{\prime \intercal} \mathbf{x} = 0 $$.
Thus, **the intersection of two lines l and l is the point $$ \mathbf{x} = \mathbf{l} × \mathbf{l}^{\prime} $$.**

Consider the simple problem of determining the intersection of the lines $$ x = 1  $$ and $$ y = 1 $$.
$$  \mathbf{l} = (−1, 0, 1)^\intercal,    \mathbf{l}'  = (0, −1, 1)^\intercal $$.

$$ \mathbf{x} = \mathbf{l} \times \mathbf{l}'
              = \begin{vmatrix} i & j & k \\
                               -1 & 0 & 1 \\
                               0 & -1 & 1 \end{vmatrix} = \begin{pmatrix} 1 \\ 1 \\ 1 \end{pmatrix} $$

which is the inhomogeneous point $$ (1, 1)^\intercal $$ as required.

**Line joining points. The line through two points $$ x $$ and $$ x $$ is $$ \mathbf{l} = x × x $$.**

**Intersection of parallel lines.**
The intersection of parallel lines $$ \mathbf{l} = (a, b, c)^\intercal $$ and
$$ \mathbf{l}' = (a, b, c')^\intercal $$ is $$ \mathbf{l} × \mathbf{l}' = (c − c) (b, −a, 0)^\intercal $$,
and ignoring the scale factor $$ (c − c) $$, this is the point $$ (b, −a, 0)^\intercal $$. 

**Ideal points and the line at infinity.**
Homogeneous vectors $$ \mathbf{x} = (x_1, x_2, x_3)^\intercal $$ with last coordinate $$ x_3 = 0 $$ are known as ideal points,
or points at infinity. The set of all **ideal** points may be written $$ (x_1, x_2, 0)T $$,
with a particular point specified by the ratio $$ x_1 : x_2 $$. Note that this set lies on a single line,
the **line at infinity**, denoted by the vector $$ l_∞ = (0, 0, 1)^\intercal $$._
Indeed, one verifies that $$ (0, 0, 1) (x_1, x_2, 0)^\intercal = 0 $$.

**A model for the projective plane.** A fruitful way of thinking of
$$ \mathbb{P}^2 $$ is as a set of rays in $$ \mathbb{R}^3 $$.
The set of all vectors $$ k(x_1, x_2, x_3)^\intercal $$ as $$ k $$ varies forms a ray through the origin.
Such a ray may be thought of as representing a single point in $$ \mathbb{P}^2 $$.
In this model, the lines in $$ \mathbb{P}^2 $$ are planes passing through the origin.
One verifies that two nonidentical rays lie on exactly one plane,
and any two planes intersect in one ray.
This is the analogue of two distinct points uniquely defining a line,
and two lines always intersecting in a point.

![mvgcv-1](https://lh3.googleusercontent.com/2KbKYhmY3UxMjYdn-3mUgTkog3pxnU99DyzTNtPZnL57N9qoRR9dn_F6IRP4XUPFO3zHANnOv7XCXw=w545-h430-no)

Points and lines may be obtained by intersecting this set of rays and planes by the plane $$ x_3 = 1 $$.
As illustrated in the figure above the rays representing ideal points
and the plane representing $$ l_\infty $$ are parallel to the plane $$ x_3 = 1 $$.
Points and lines of $$ \mathbb{P}^2 $$ are represented by rays and planes,
respectively, through the origin in $$ \mathbb{R}^3 $$.
Lines lying in the $$ x_1 x_2-plane $$ represent ideal points, and the $$ x_1 x_2-plane $$ represents $$ l_\infty $$.

_(HopingTang: That's tell us, why the combination of points in infinity form the line in infinity.)


**Duality principle.**
To any theorem of 2-dimensional projective geometry there corresponds a dual theorem,
which may be derived by interchanging the roles of points and lines in the original theorem.

**Conic.**
The equation of a conic in inhomogeneous coordinates is

$$ ax^2 + bxy + cy^2 + dx + ey + f = 0 $$

i.e. a polynomial of degree 2. “Homogenizing” this by the replacements: $$ x → x_1/x_3, y → x_2/x_3 $$ gives

$$ a x_1^2 + b x_1 x_2 + c x_2^2 + d x_1 x_3 + e x_2 x_3 + f x_3^2 = 0 $$

or in matrix form

$$ \mathbf{x}^\intercal \mathbf{C} \mathbf{x} = 0 $$

where the conic coefficient matrix C is given by

$$ \mathbf{C} = \begin{bmatrix} a   &   b/2   & d/2 \\
                               b/2  &    c    & e/2 \\
                               d/2  &   c/2   &  f  \end{bmatrix}  $$

Note that the conic coefficient matrix is symmetric.
Multiplying $$ \mathbf{C} $$ by a non-zero scalar does not affect the above equations.
Thus $$ \mathbf{C} $$  is a homogeneous representation of a conic.
The conic has five degrees of freedom which can be thought of as the ratios $$ \{a : b : c : d : e : f\} $$
or equivalently the six elements of a symmetric matrix less one for scale.

**Tangent lines to conics.
The line $$ \mathbf{l} $$ tangent to $$ \mathbf{C} $$ at a point $$ \mathbf{x} $$ 
on $$ \mathbf{C} $$ is given by $$ \mathbf{l} = \mathbf{C} \mathbf{x} $$ .**

**Dual conics.** A line $$ \mathbf{l} $$ tangent to the conic $$\mathbf{C} $$ satisfies
$$ \mathbf{l}^\intercal \mathbf{C}^∗ \mathbf{l} = 0 $$.
The notation $$ C^∗ $$ indicates that $$ C^∗ $$ is the adjoint matrix of $$ C $$.
For a non-singular symmetric matrix $$ C^∗ = C^{−1} $$ (up to scale). Thus, $$ \mathbf{l}^\intercal \mathbf{C}^{−1} \mathbf{l} = 0 $$.
The equation for a dual conic is straightforward to derive in the case that C has full rank. 
The conic $$ \mathbf{C} $$ is the **envelope** of the lines $$ l $$.

**Degenerate conics.**
If the matrix $$ \mathbf{C} $$ is not of full rank, then the conic is termed degenerate.
Degenerate point conics include two lines (rank 2), and a repeated line (rank 1).
**Degenerate line conics** include two points (rank 2), and a repeated point (rank 1).
For example, the line conic $$ \mathbf{C}^∗ = \mathbf{x} \mathbf{y}^\intercal + \mathbf{y} \mathbf{x}^\intercal $$ has rank 2
and consists of lines passing through either of the two points $$ x $$ and $$ y $$.
Note that for matrices that are not invertible $$ (\mathbf{C}^∗)^∗ \neq \mathbf{C} $$.

### 2.3 Projective transformations
A **projectivity** is an invertible mapping $$ h $$ from $$ \mathbb{P}^2 $$ to itself
such that three points $$ x_1 $$, $$ x_2 $$ and $$ x_3 $$ lie on the same line
if and only if $$ h(x_1) $$, $$ h(x_2) $$ and $$ h(x_3) $$ do.
Projectivities form a group since the inverse of a projectivity is also a projectivity,
and so is the composition of two projectivities.
A projectivity is also called a **collineation** (a helpful name),
a **projective transformation** or a **homography**: the terms are synonymous.

**A mapping $$ h : \mathbb{P}^2 → \mathbb{P}^2 $$ is a projectivity
if and only if there exists a non-singular $$ 3 × 3 $$ matrix $$ \mathbf{H} $$
such that for any point in $$ \mathbb{P}^2 $$ 
represented by a vector $$ x $$ it is true that $$ h(x) = \mathbf{H}x $$.**

**Projective transformation.**
A planar projective transformation is a linear transformation on homogeneous $$ 3 $$-vectors
represented by a non-singular $$ 3 × 3 $$ matrix:

$$ \begin{pmatrix} x_1' \\ x_2' \\ x_3' \end{pmatrix} = 
   \begin{bmatrix} h_{11} & h_{12} & h_{13} \\ h_{11} & h_{12} & h_{13} \\ h_{11} & h_{12} & h_{13} \end{bmatrix}
   \begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix}$$

or more briefly, x = Hx.
H is a homogeneous matrix.
There are eight independent ratios amongst the nine elements of H,
and it follows that a projective transformation has eight degrees of freedom.

If the two coordinate systems defined in the two planes are both Euclidean (rectilinear) coordinate systems
then the mapping defined by central projection is more restricted
than an arbitrary projective transformation.
It is called a **perspectivity** rather than a full projectivity,
and may be represented by a transformation with **six degrees of freedom**.

**Transformation of lines.**
$$ \mathbf{l} ^\intercal \mathbf{x}_i' = \mathbf{l} ^\intercal \mathbf{H}^{−1} \mathbf{H} \mathbf{x}_i = 0 $$.
Under the point transformation $$ \mathbf{x'} = \mathbf{H} \mathbf{x} $$, a line transforms as

$$ \mathbf{l'} = \mathbf{H}^{−\intercal} \mathbf{l} $$.

Points transform according to $$ \mathbf{H} $$, whereas lines (as rows) transform according to $$ \mathbf{H}^{−1} $$.
Points transform **contravariantly** and lines transform **covariantly**.

**Transformation of conics. Under a point transformation $$ \mathbf{x'} = \mathbf{H} \mathbf{x} $$,** becomes
$$ \mathbf{x}^\intercal \mathbf{C}\mathbf{x} 
= \mathbf{x'}^\intercal [\mathbf{H}^{−1} ] ^\intercal \mathbf{C}\mathbf{H}^{−1} \mathbf{x'}
= \mathbf{x}^\intercal \mathbf{H}^{− \intercal} \mathbf{C}\mathbf{H}^{−1} \mathbf{x} $$
**a conic $$ \mathbf{C} $$ transforms to $$ \mathbf{C'} = \mathbf{H}^{− \intercal} \mathbf{C}\mathbf{H}^{−1} $$.
a dual conic $$ \mathbf{C}^∗ $$ transforms to
$$ \mathbf{C}^{∗ \prime}= \mathbf{H}\mathbf{C}^∗  \mathbf{H}^\intercal  $$.**

### 2.4 A hierarchy of transformations
Projective transformations form a group, called the **projective linear group**,
and it will be seen that these specializations are **subgroups** of this group.

The group of invertible $$ n × n $$ matrices with real elements
is the (real) general linear group on n dimensions, or $$ GL(n) $$.
To obtain the projective linear group the matrices related by a scalar multiplier are identified,
giving $$ PL(n) $$ (this is a quotient group of $$ GL(n) $$).
In the case of projective transformations of the plane $$ n = 3 $$.
The important subgroups of $$ PL(3) $$ include the affine group,
which is the subgroup of $$ PL(3) $$ consisting of matrices
for which the last row is $$ (0, 0, 1) $$,
and the Euclidean group, which is a subgroup of the affine group
for which in addition the upper left hand $$ 2 × 2 $$ matrix is orthogonal.
One may also identify the oriented Euclidean group
in which the upper left hand $$ 2 × 2 $$ matrix has determinant $$ 1 $$.

**Class I: Isometries** are transformations of the plane \mathbb{R}^2 that preserve Euclidean distance
(from iso = same, metric = measure). An isometry is represented as

$$ \begin{pmatrix} x' \\ y' \\ 1 \end{pmatrix}
= \begin{bmatrix} \epsilon \cos \theta & - \sin \theta & t_x \\
                  \epsilon \sin \theta &   \cos \theta & t_y \\
                  0                    &   0           & 1  \end{bmatrix}
  \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} $$

where $$ \epsilon = ±1 $$.
If $$ \epsilon = 1 $$ then the isometry is **orientation-preserving** and is a **Euclidean transformation**
(a composition of a translation and rotation).
If $$ \epsilon = −1 $$ then the isometry reverses orientation.
A planar Euclidean transformation can be written more concisely in block form as

$$ \mathbf{x'} = \mathbf{H}_E \mathbf{x}
= \begin{bmatrix} \mathbf{R} & \mathbf{t} \\ \mathbf{O} ^\intercal & 1 \end{bmatrix} \mathbf{x} $$

where_ $$ \mathbf{R} $$ is a $$ 2 × 2 $$ rotation matrix
(an orthogonal matrix such that
$$ \mathbf{R} ^\intercal  \mathbf{R} = \mathbf{R}\mathbf{R}^\intercal  = \mathbf{I} $$),
t a translation $$ 2 $$-vector, and $$ \mathbf{0} $$ a null 2-vector.
Special cases are a pure rotation (when $$ t = 0 $$ )
and a pure translation (when $$ \mathbf{R} = \mathbf{I} $$).
A Euclidean transformation is also known as a **displacement**.

A planar Euclidean transformation has three degrees of freedom, 
one for the rotation and two for the translation. 
The transformation can be computed from two point correspondences.

Invariants: length (the distance between two points), angle (the angle between two lines), and area.

**Class II: A Similarity transformations** (or more simply a similarity) is an isometry
composed with an isotropic scaling.
In the case of a Euclidean transformation composed with a scaling (i.e. no reflection)
the similarity has matrix representation

$$ \begin{pmatrix} x' \\ y' \\ 1 \end{pmatrix}
= \begin{bmatrix} s \cos \theta & - s \sin \theta & t_x \\
                  s \sin \theta &   s \cos \theta & t_y \\
                  0             &   0        & 1  \end{bmatrix}
  \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} $$

This can be written more concisely in block form as

$$ \mathbf{x'} = \mathbf{H}_S \mathbf{x}
= \begin{bmatrix} s \mathbf{R} & \mathbf{t} \\ \mathbf{O} ^\intercal & 1 \end{bmatrix} \mathbf{x} $$

where_ the scalar s represents the isotropic scaling.
A similarity transformation is also known as an **equi-form** transformation,
because it preserves “shape” (form). A planar similarity transformation has four degrees of freedom,
the scaling accounting for one more degree of freedom than a Euclidean transformation.
A similarity can be computed from two point correspondences.

**Invariants** : Angles, parallel, the ratio of two lengths (ratio of areas).

The description **metric structure** implies that the structure is defined up to a similarity.

**Class III: An affine transformation** (or more simply an affinity) is a non-singular linear
transformation followed by a translation. It has the matrix representation

$$ \begin{pmatrix} x' \\ y' \\ 1 \end{pmatrix} 
= \begin{bmatrix} a_{11} & a_{12} & t_x \\
                  a_{21} & a_{22} & t_y \\
                  0      &   0    & 1  \end{bmatrix}
  \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} $$

or in block form

$$ \mathbf{x'} = \mathbf{H}_A \mathbf{x}
= \begin{bmatrix} \mathbf{A} & \mathbf{t} \\ \mathbf{O} ^\intercal & 1 \end{bmatrix} \mathbf{x} $$


with_  $$ \mathbf{A} $$ a $$ 2 × 2 $$ non-singular matrix.
A planar affine transformation has six degrees of freedom corresponding to the six matrix elements.
The transformation can be computed from three point correspondences.
The affine matrix A can always be decomposed (SVD) as

$$ \mathbf{A}  = \mathbf{U}\mathbf{D}\mathbf{V}^\intercal
= (\mathbf{U}\mathbf{V}^\intercal )(\mathbf{V}\mathbf{D}\mathbf{V}^\intercal )
= R(θ) R(−φ) D R(φ) $$

where $$ \mathbf{U} $$ and $$ \mathbf{V} $$ are orthogonal matrices,
$$ R(θ) $$ and $$ R(φ) $$ are rotations by $$ θ $$ and $$ φ $$ respectively,
and $$ \mathbf{D} $$ is a diagonal matrix:

$$ \mathbf{D} = \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} $$

The affine matrix $$ \mathbf{A} $$ is hence seen to be the concatenation of a rotation (by $$ φ $$);
a scaling by $$ λ_1 $$ and $$ λ_2 $$ respectively in the (rotated) $$ x $$ and $$ y $$ directions;
a rotation back (by $$ −φ $$); and finally another rotation (by $$ θ $$).

**Invariants** :  Parallel lines, Ratio of lengths of parallel line segments, Ratio of areas.

**Class IV: A Projective transformations** was defined in section 2.3.
It is a general non-singular linear transformation of homogeneous coordinates. Here we examine its block form

$$ \mathbf{x'} = \mathbf{H}_P \mathbf{x}
= \begin{bmatrix} \mathbf{A} & \mathbf{t} \\ \mathbf{v} ^\intercal & v \end{bmatrix} \mathbf{x} $$

where_ the vector $$ v = (v_1, v_2)^\intercal  $$.
A projective transformation between two planes can be computed from four point correspondences,
with no three collinear on either plane.

**Invariants**:  a ratio of ratios or cross ratio of lengths on a line is a projective invariant.

Decomposition of a projective transformation:

$$ \mathbf{H} = \mathbf{H}_S \mathbf{H}_A \mathbf{H}_P
= \begin{bmatrix} s \mathbf{R} & \mathbf{t} \\ \mathbf{O} ^\intercal & 1 \end{bmatrix}
 \begin{bmatrix} \mathbf{K} & \mathbf{O} \\ \mathbf{O} ^\intercal & 1 \end{bmatrix}
 \begin{bmatrix} \mathbf{I} & \mathbf{O} \\ \mathbf{v} ^\intercal & v \end{bmatrix}
= \begin{bmatrix} \mathbf{A} & \mathbf{t} \\ \mathbf{v} ^\intercal & v \end{bmatrix} $$

with_ $$ \mathbf{A} $$ a non-singular matrix given by 
$$ \mathbf{A} = s\mathbf{R}\mathbf{K}+\mathbf{t}\mathbf{v}^\intercal  $$,
and K an upper-triangular matrix normalized as $$ \det \mathbf{K} = 1 $$.
This decomposition is valid provided $$ v \neq 0 $$, and is unique if $$ s $$ is chosen positive.
The transformation HP is an **elation**, described in section A7.3(p631).

**The number of functionally independent invariants is equal to, or greater than,
the number of degrees of freedom of the configuration less the number of degrees of freedom of the transformation.**

![mvgcv-002](https://lh3.googleusercontent.com/i7wRr_OJkUwlwKZviiOLzm_C7ri3Q0aj2FwKbiqxB4xQfKbDfvT_v7BRerTZeSDwOdWhTtbqWHNt-w=w998-h557-no)
