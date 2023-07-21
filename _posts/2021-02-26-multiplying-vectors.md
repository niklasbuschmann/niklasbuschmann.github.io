---
title: "Multiplying vectors: An introduction to geometric algebra"
date: 2021-02-26
description: An introduction to geometric algebra
---

In this post we will start in two dimensions and derive the scalar product and two-dimensional analog of the cross product, assuming only that they are invariant under rotations of the coordinate system. 

We will then derive a set of rules that define something named [geometric algebra](https://en.wikipedia.org/wiki/Geometric_algebra) which allows us to generalize the concept to any number of dimensions. A more detailed description of the resulting two-dimensional geometric algebra can be found in Joe Gregorio's [Introduction to Geometric Algebra over R^2](https://bitworking.org/news/ga/2d/).

### Table of contents

- [Rotational invariance](#part-1-rotational-invariance)
- [Geometric Algebra](#part-2-geometric-algebra)
- [Three dimensions](#part-3-three-dimensions)
   - [Quaternions](#quaternions)
   - [Electromagnetism](#electromagnetism)
- [Four dimensions](#part-4-four-dimensions)
- [Scalar and wedge product](#part-5-scalar-and-wedge-product)
- [Matrix representations](#part-6-matrix-representations)
- [Summary](#summary)

## Part 1: Rotational Invariance

For the following we define a set of two basis vectors:

$$
  \mathbf{e}_x := \begin{pmatrix}1\\0\end{pmatrix} \qquad \mathbf{e}_y := \begin{pmatrix}0\\1\end{pmatrix}
$$

This allows us to write any ordinary vector \\( \mathbf{a} \\) as a combination of these basis vectors:

$$
  \mathbf{a} = \begin{pmatrix}a_1\\a_2\end{pmatrix} = a_1\begin{pmatrix}1\\0\end{pmatrix} + a_2\begin{pmatrix}0\\1\end{pmatrix} = a_1 \mathbf{e}_x+a_2 \mathbf{e}_y
$$

One fundamental property associated with multiplication is [distributivity](https://en.wikipedia.org/wiki/Distributive_property) over addition:

$$
  (\mathbf{a}+\mathbf{b})\mathbf{c} = \mathbf{a}\mathbf{c} + \mathbf{b}\mathbf{c} \qquad \mathbf{c}(\mathbf{a}+\mathbf{b})=\mathbf{c}\mathbf{a}+\mathbf{c}\mathbf{b}
$$

This property is so fundamental that if the operation is not distributive it probably should not be called multiplication. This reduces the problem of multiplying to vectors to the problem of multiplying the basis vectors:

$$
\begin{aligned}
  \mathbf{a}\mathbf{b}
  &= (a_1 \mathbf{e}_x+a_2 \mathbf{e}_y)(b_1 \mathbf{e}_x+b_2 \mathbf{e}_y)\\[1ex]
  &= a_1b_1\mathbf{e}_x\mathbf{e}_x+a_1b_2\mathbf{e}_x\mathbf{e}_y+a_2b_1\mathbf{e}_y\mathbf{e}_x+a_2b_2\mathbf{e}_y\mathbf{e}_y
\end{aligned}
$$

Although not very useful yet, this is the most general form of what the product of two vectors should look like.

One desirable property of our multiplication would be that the product of two vectors should only depend on the relative angle between them, not on the absolute angles of the vectors themselves. In other words: the product should stay invariant under rotations of the coordinate system. (This also implies that our result is not a vector - which is not invariant under rotations.)

Let us now consider for example the product of a unit vector \\( \mathbf{\hat{u}} \\) with itself: We could then choose three coordinate systems where \\( \mathbf{\hat{u}} \\) points (i) along the x-axis or (ii) somewhere between x and y-axis or (iii) along the y-axis. Calculating the results and requiring them to be equal yields:

$$
  \mathbf{e}_x\mathbf{e}_x = u_1^2 \mathbf{e}_x\mathbf{e}_x + u_1u_2 \mathbf{e}_x\mathbf{e}_y + u_2u_1\mathbf{e}_y\mathbf{e}_x + u_2^2 \mathbf{e}_y\mathbf{e}_y = \mathbf{e}_y\mathbf{e}_y
$$

It immediately follows that \\( \mathbf{e}_x\mathbf{e}_x = \mathbf{e}_y\mathbf{e}_y \\), and combined with the fact that \\( \\|\mathbf{\hat{u}}\\|^2 = u_1^2+u_2^2 = 1 \\) this means that \\( \mathbf{e}_x\mathbf{e}_y \\) and \\( \mathbf{e}_y\mathbf{e}_x \\) must cancel out each other. We will use this to define two new basis vectors:

$$
  \mathbf{e}_s := \mathbf{e}_x\mathbf{e}_x = \mathbf{e}_y\mathbf{e}_y \qquad \mathbf{e}_c := \mathbf{e}_x\mathbf{e}_y = -\mathbf{e}_y\mathbf{e}_x
$$

The product can then be expressed as:

$$
\begin{aligned}
  \mathbf{a}\mathbf{b}
  &= (a_1 b_1 + a_2 b_2)\mathbf{e}_s + (a_1b_2 - a_2b_1)\mathbf{e}_c \\[1ex]
  &= (\mathbf{a}\cdot\mathbf{b})\mathbf{e}_s + (\mathbf{a}\times\mathbf{b})\mathbf{e}_c
\end{aligned}
$$

The resulting multiplication closely resembles the definition of the [dot product](https://en.wikipedia.org/wiki/Dot_product) and the [cross product](https://en.wikipedia.org/wiki/Cross_product) - if we consider the cross product of two vectors in the xy-plane to be the z-component of the traditional cross product in three dimensions: \\(\ \mathbf{a}\times\mathbf{b} := a_1b_2 - a_2b_1 \\).

Both scalar and cross product - restricted to one plane - are invariant under rotations, so the two requirements we found previously are sufficient to guarantee rotational invariance for the product of two arbitrary vectors. Thus the product can not be simplified further without losing information.

## Part 2: Geometric Algebra

We found two fundamental types of multiplication - one that stays the same when the vectors are switched and one that changes its sign. Now we can try to see how \\( \mathbf{e}_s \\) and \\( \mathbf{e}_c \\) multiply with each other:

$$
  \mathbf{e}_s \mathbf{e}_s = \mathbf{e}_x \mathbf{e}_x \mathbf{e}_y \mathbf{e}_y = - \mathbf{e}_x \mathbf{e}_y \mathbf{e}_x \mathbf{e}_y = -\mathbf{e}_c\mathbf{e}_c \\[1ex]
  \quad \mathbf{e}_s \mathbf{e}_c = \mathbf{e}_x \mathbf{e}_x \mathbf{e}_x \mathbf{e}_y = -\mathbf{e}_x \mathbf{e}_x \mathbf{e}_y \mathbf{e}_x = \mathbf{e}_x \mathbf{e}_y \mathbf{e}_x \mathbf{e}_x = \mathbf{e}_c \mathbf{e}_s
$$

(Note that here we also implicitly assumed [associativity](https://en.wikipedia.org/wiki/Associative_property), requiring that \\( \mathbf{a}(\mathbf{b}\mathbf{c})=(\mathbf{a}\mathbf{b})\mathbf{c}\\))

The product of an arbitrary combination of \\( \mathbf{e}_s \\) and \\( \mathbf{e}_c \\) can thus be written as:

$$
  (a_1\mathbf{e}_s + a_2\mathbf{e}_c)(b_1\mathbf{e}_s + b_2\mathbf{e}_c) = (a_1b_1 - a_2b_2)\mathbf{e}_s\mathbf{e}_s + (a_1b_2 + a_2b_1)\mathbf{e}_s\mathbf{e}_c
$$

Let us now look at rotations of the coordinate system (e.g. \\(\mathbf{e}_x \mapsto \mathbf{e}_y, \mathbf{e}_y \mapsto -\mathbf{e}_x\\)) and reflections (e.g. \\(\mathbf{e}_x \mapsto \mathbf{e}_y, \mathbf{e}_y \mapsto \mathbf{e}_x\\)). We chose \\( \mathbf{e}_s \\) and \\( \mathbf{e}_c \\) in a way that left them unchanged when the coordinates are rotated. We can also see that \\( \mathbf{e}_s \\) stays the same when reflected, whereas \\( \mathbf{e}_c \\) changes its sign under reflection. This means \\(\mathbf{e}_s\\) is a [scalar](https://en.wikipedia.org/wiki/Scalar) and \\( \mathbf{e}_c \\) is a [pseudoscalar](https://en.wikipedia.org/wiki/Pseudoscalar).

We can also recognize that \\( \mathbf{e}_s\mathbf{e}_s \\) is another scalar and \\( \mathbf{e}_s\mathbf{e}_c \\) is another pseudoscalar. Assuming there only exists one kind of scalar and one kind of pseudoscalar, we will assure that \\( \mathbf{e}_s=\mathbf{e}_s\mathbf{e}_s \\) and \\( \mathbf{e}_c=\mathbf{e}_s\mathbf{e}_c \\) by defining \\( \mathbf{e}_s \\) to be the [neutral element](https://en.wikipedia.org/wiki/Identity_element), that when multiplied with leaves everything unchanged.

With this \\( \\{\mathbf{e}_s, \mathbf{e}_c\\} \\) is now [isomorphic](https://en.wikipedia.org/wiki/Isomorphism) to the [complex numbers](https://en.wikipedia.org/wiki/Complex_number) \\( \\{1, i\\} \\), meaning both behave completely identical with respect to addition and multiplication, and allowing us to rename \\( \mathbf{e}_s = 1 \\) and \\(\mathbf{e}_c = I \\). Note that a capital \\( I \\) is used here to distinguish vectors with complex coefficients.

Since they are isomorphic to the complex numbers, [Euler's formula](https://en.wikipedia.org/wiki/Euler's_formula) also applies to \\( 1 \\) and \\( I \\):

$$
  e^{I \varphi} = \cos(\varphi) + \sin(\varphi) I
$$

By definition we know that vectors are left unchanged when multiplied with the neutral element \\( 1 \\). But how does \\( I \\) act on the ordinary basis vectors?

$$
  I \mathbf{e}_x = \mathbf{e}_x \mathbf{e}_y \mathbf{e}_x = - \mathbf{e}_y \\[1ex]
  I \mathbf{e}_y = \mathbf{e}_x \mathbf{e}_y \mathbf{e}_y = + \mathbf{e}_x
$$

From this one can see that \\( I \\) acts like a rotor on any vector. Multiplying a vector with \\( a + b I \\) then follows the rules known from [complex numbers](https://en.wikipedia.org/wiki/Complex_numbers#Multiplication_and_division_in_polar_form).

Because \\( \\{1, I\\} \\) is closed under multiplication and both give back a vector when multiplied with a vector, the complete geometric algebra \\( \\{1, \mathbf{e}_x, \mathbf{e}_y, I\\} \\) is closed under multiplication, with the following multiplication table:

$$
  \mathbf{e}_x\mathbf{e}_x = 1 \quad \mathbf{e}_x\mathbf{e}_y = I \quad \mathbf{e}_x I = \mathbf{e}_y\\[1ex]
  \mathbf{e}_y\mathbf{e}_x = - I \quad \mathbf{e}_y\mathbf{e}_y = 1 \quad \mathbf{e}_y I = -\mathbf{e}_x\\[1ex]
  I\mathbf{e}_x = -\mathbf{e}_y \quad I\mathbf{e}_y=\mathbf{e}_x \quad I^2 = -1
$$

Together with associativity and distributivity, this can be simplified to the two following rules:

$$
  1=\mathbf{e}_x\mathbf{e}_x = \mathbf{e}_y \mathbf{e}_y \qquad I=\mathbf{e}_x\mathbf{e}_y = -\mathbf{e}_y\mathbf{e}_x
$$

Note that defining the squares \\( \mathbf{e}_x\mathbf{e}_x \\) and \\( \mathbf{e}_y\mathbf{e}_y \\) to be \\( 1 \\) is important since this implies that they are the [neutral element](https://en.wikipedia.org/wiki/Identity_element), whereas \\( I \\) is just another name for \\( \mathbf{e}_x\mathbf{e}_y \\) and will change in higher dimensions.

In fact the geometric algebra can be generalized to any number of dimensions where for any \\( i \neq j \\):

$$
  \mathbf{e}_i\mathbf{e}_i = 1 \qquad \mathbf{e}_i\mathbf{e}_j = -\mathbf{e}_j\mathbf{e}_i
$$

Using the [anti-commutator](https://en.wikipedia.org/wiki/Commutator#Ring_theory) and the [delta symbol](https://en.wikipedia.org/wiki/Kronecker_delta) this can be written more elegantly as:

$$
  \{\mathbf{e}_i,\mathbf{e}_j\} = 2\delta_{ij}
$$

## Part 3: Three Dimensions

In three dimensions there exist eight basis vectors:

$$
  \{1, \mathbf{e}_x, \mathbf{e}_y, \mathbf{e}_z, \mathbf{e}_y\mathbf{e}_z, \mathbf{e}_z\mathbf{e}_x, \mathbf{e}_x\mathbf{e}_y, \mathbf{e}_x\mathbf{e}_y\mathbf{e}_z \}
$$

Again defining the pseudoscalar \\( I:=\mathbf{e}_x\mathbf{e}_y\mathbf{e}_z\\) allows the basis to be rewritten as:

$$
  \{1, \mathbf{e}_x, \mathbf{e}_y, \mathbf{e}_z, I\mathbf{e}_x, I\mathbf{e}_y, I\mathbf{e}_z, I \}
$$

Where we used that:

$$
  I\mathbf{e}_x = \mathbf{e}_y \mathbf{e}_z \qquad
  I\mathbf{e}_y = \mathbf{e}_z \mathbf{e}_x \qquad
  I\mathbf{e}_z = \mathbf{e}_x \mathbf{e}_y
$$

Let us see what happens when two ordinary vectors are multiplied together:

$$
  \mathbf{a}\mathbf{b} = (a_1b_1 + a_2b_2+a_3b_3) + (a_1b_2-a_2b_1)\mathbf{e}_x\mathbf{e}_y - (a_1b_3-a_3b_1)\mathbf{e}_z\mathbf{e}_x + (a_2b_3-a_3b_2)\mathbf{e}_y\mathbf{e}_z
$$

Using the pseudoscalar \\( I \\) this can again be written as:

$$
  \mathbf{a}\mathbf{b} = \mathbf{a}\cdot\mathbf{b} + (\mathbf{a}\times\mathbf{b}) I
$$

Another interesting property is that the pseudoscalar part of the product of three vectors equals the [determinant](https://en.wikipedia.org/wiki/Determinant) or [triple product](https://en.wikipedia.org/wiki/Triple_product) of them:

$$
\begin{aligned}
  \mathbf{a}\mathbf{b}\mathbf{c}
  &= (\mathbf{a}\cdot\mathbf{b})\mathbf{c} + ((\mathbf{a}\times\mathbf{b})\cdot\mathbf{c}+((\mathbf{a}\times\mathbf{b})\times\mathbf{c}) I) I\\[1ex]
  &= (\mathbf{a}\cdot\mathbf{b})\mathbf{c} - (\mathbf{a}\cdot\mathbf{c})\mathbf{b} + (\mathbf{b}\cdot\mathbf{c})\mathbf{a} + \det([\mathbf{a}|\mathbf{b}|\mathbf{c}]) I
\end{aligned}
$$

### Quaternions

Like in the two-dimensional case, the pseudovectors together with the scalar are closed under multiplication:

$$
  (\mathbf{e}_x\mathbf{e}_y)^2 = (\mathbf{e}_z\mathbf{e}_x)^2 = (\mathbf{e}_y\mathbf{e}_z)^2 = -1 \\[2ex]
  (\mathbf{e}_x\mathbf{e}_y)(\mathbf{e}_z\mathbf{e}_x) = \mathbf{e}_y\mathbf{e}_z \qquad (\mathbf{e}_z\mathbf{e}_x)(\mathbf{e}_y\mathbf{e}_z) = \mathbf{e}_x\mathbf{e}_y \qquad (\mathbf{e}_y\mathbf{e}_z)(\mathbf{e}_x\mathbf{e}_y) = \mathbf{e}_z\mathbf{e}_x
$$

Together \\( \\{1, \mathbf{e}_x\mathbf{e}_y, \mathbf{e}_z\mathbf{e}_x, \mathbf{e}_y\mathbf{e}_z\\} \\) are isomorphic to the [quaternions](https://en.wikipedia.org/wiki/Quaternion) \\( \\{1, i, j, k\\} \\), and when multiplicated with ordinary vectors are quite useful as [three-dimensional rotors](https://en.wikibooks.org/wiki/Physics_Using_Geometric_Algebra/Basic_Geometric_Algebra#Rotation_(G3)).

### Electromagnetism

Another nice property of geometric algebra is that [Maxwell's equations](https://en.wikipedia.org/wiki/Maxwell%27s_equations) take on a particularly simple form when [expressed](https://en.wikipedia.org/wiki/Mathematical_descriptions_of_the_electromagnetic_field#Geometric_algebra_formulations) using it:

$$
  \left(\frac{1}{c}\frac{\partial}{\partial t} + \mathbf{\nabla}\right)\left(\mathbf{E}+ c\mathbf{B}I\right) = \frac{\rho}{\epsilon_0} - \mu_0c\mathbf{j}
$$

Expanding the product shows that the expression above is in fact equal to Maxwell's equations:

$$
  \underbrace{\left(\mathbf{\nabla}\cdot\mathbf{E}\right)}_\mathrm{Gauss} + 
  c\underbrace{\left(\frac{1}{c^2}\frac{\partial \mathbf{E}}{\partial t} - \mathbf{\nabla}\times\mathbf{B}\right)}_\mathrm{Ampere-Maxwell} + 
  \underbrace{\left(\mathbf{\nabla}\times\mathbf{E}+\frac{\partial \mathbf{B}}{\partial t}\right)}_\mathrm{Faraday} I + 
  c\underbrace{\left(\mathbf{\nabla}\cdot\mathbf{B}\right)}_\mathrm{Gauss} I
  = \frac{\rho}{\epsilon_0} - \mu_0c\mathbf{j}
$$

Using the [electric potential](https://en.wikipedia.org/wiki/Electric_potential) \\( \phi \\) and [magnetic potential](https://en.wikipedia.org/wiki/Magnetic_vector_potential) \\( \mathbf{A} \\) - and using the [Lorenz gauge](https://en.wikipedia.org/wiki/Lorenz_gauge_condition) - we can write:

$$
  \left(\frac{1}{c}\frac{\partial}{\partial t} - \mathbf{\nabla}\right)\left(\phi-c\mathbf{A}\right)
  = c\underbrace{\left(\frac{1}{c^2}\frac{\partial \phi}{\partial t}+\mathbf{\nabla} \cdot \mathbf{A}\right)}_{=\ 0}
  + \underbrace{\left(-\mathbf{\nabla}\phi - \frac{\partial \mathbf{A}}{\partial t}\right)}_{=\ \mathbf{E}} + c\underbrace{\left(\mathbf{\nabla} \times \mathbf{A}\right)}_{=\ \mathbf{B}}I
$$

This yields the known formular involving the [d'Alembert operator](https://en.wikipedia.org/wiki/D%27Alembert_operator):

$$
  \left(\frac{1}{c^2}\frac{\partial^2}{\partial t^2} - \nabla^2\right)\left(\phi-c\mathbf{A}\right) = \frac{\rho}{\epsilon_0} - \mu_0c\mathbf{j}
$$

The Lorentz force law can be expressed using the four velocity \\( c + \mathbf{v} \\) and gives the result for the power and force experienced by both electric and magnetic monopoles:

$$
  (\mathbf{E}+c\mathbf{B}I)(qc+q\mathbf{v})
  = \underbrace{q(\mathbf{E}\cdot\mathbf{v})}_{\textrm{power}} + c\underbrace{q(\mathbf{E} - \mathbf{B}\times\mathbf{v})}_{\textrm{electric force}} + c^2\underbrace{q\left(\mathbf{B} + \mathbf{E}\times\frac{\mathbf{v}}{c^2}\right)}_{\textrm{magnetic force}} I + c\underbrace{q(\mathbf{B}\cdot\mathbf{v})}_{\textrm{power}}I
$$

## Part 4: Four Dimensions

In four dimensions there are now 16 basis vectors:

$$
  \{1, \mathbf{e}_t, \mathbf{e}_x, \mathbf{e}_y, \mathbf{e}_z, \mathbf{e}_t\mathbf{e}_x, \mathbf{e}_t\mathbf{e}_y, \mathbf{e}_t\mathbf{e}_z, \mathbf{e}_x\mathbf{e}_y, \mathbf{e}_z\mathbf{e}_x, \mathbf{e}_y\mathbf{e}_z, I\mathbf{e}_t, I\mathbf{e}_x, I\mathbf{e}_y, I\mathbf{e}_z, I \}
$$

Since there are now six bivector components, the product of two vectors does not again correspond to a four-component vector. Because of this, the cross product, generalized to four dimensions, would either return six components or require three vectors as input.

Working with four dimensions usually involves space-time, so I decided to name the added dimension \\( \mathbf{e}_t\\). And because space-time tends to be coupled to special relativity we will now switch to a different set of basis vectors:

$$
  \bm{\gamma}_0 := \mathbf{e}_t \qquad \bm{\gamma}_1 := \mathbf{e}_t\mathbf{e}_x \qquad \bm{\gamma}_2 := \mathbf{e}_t\mathbf{e}_y \qquad \bm{\gamma}_3 := \mathbf{e}_t\mathbf{e}_z
$$

One can verify that this again defines a geometric algebra - called [spacetime algebra](https://en.wikipedia.org/wiki/Spacetime_algebra) - where now \\( \bm{\gamma}_1, \bm{\gamma}_2, \bm{\gamma}_3\\) square to \\( -1 \\) instead of \\( 1 \\), requiring modified multiplication rules. Using the metric tensor \\( \eta\_{\mu\nu} \\) with signature \\( (+, -, -, -) \\) these new rules can be expressed as:

$$
  \{\bm{\gamma}_\mu,\bm{\gamma}_\nu\} = 2\eta_{\mu\nu}
$$

One [consequence](https://en.wikipedia.org/wiki/Spacetime_algebra#Spacetime_split) of this is that when a spacetime vector is multiplied by \\( \bm{\gamma}_0 \\), the result behaves like an ordinary 3d vector plus a scalar equivalent to the time dimension.

Another interesting property is that in the spacetime algebra there exists a "square root" of the d'Alembert operator:

$$
  \left(\bm{\gamma}_0 \frac{1}{c}\frac{\partial}{\partial t} + \bm{\gamma}_1 \frac{\partial}{\partial x} + \bm{\gamma}_2 \frac{\partial}{\partial y} + \bm{\gamma}_3 \frac{\partial}{\partial z} \right)^2 = \frac{1}{c^2}\frac{\partial^2}{\partial t^2} - \nabla^2
$$

Now, since the [octonions](https://en.wikipedia.org/wiki/Octonion) are not associative, there does not exist a subset of basis vectors isomorphic to them. But the complete three dimensional geometric algebra is isomorphic to the [biquaternions](https://en.wikipedia.org/wiki/Biquaternion), in four dimensions the scalar, bivectors and pseudoscalar together are isomorphic to the [split-biquaternions](https://en.wikipedia.org/wiki/Split-biquaternion) and with spacetime the scalar, bivectors and pseudoscalar together are isomorphic to the [split-octonions](https://en.wikipedia.org/wiki/Split-octonion). The "split" comes from the fact that in four dimensions \\( I^2 = (\mathbf{e}_t\mathbf{e}_x\mathbf{e}_y\mathbf{e}_z)^2 = 1 \\).

## Part 5: Scalar and Wedge product

In three dimensions the geometric prouct can be written as:

$$
  \mathbf{a}\mathbf{b} = \mathbf{a}\cdot\mathbf{b} + (\mathbf{a}\times\mathbf{b}) I \quad \Leftrightarrow \quad \mathbf{b}\mathbf{a} = \mathbf{a}\cdot\mathbf{b} - (\mathbf{a}\times\mathbf{b}) I
$$

But in any number of dimensions the product between two vectors splits into a commutative and anticommutative part, called scalar and [wedge product](https://en.wikipedia.org/wiki/Exterior_algebra):

$$
  \mathbf{a}\mathbf{b} = \mathbf{a}\cdot\mathbf{b} + \mathbf{a}\wedge\mathbf{b} \quad \Leftrightarrow \quad \mathbf{b}\mathbf{a} = \mathbf{a}\cdot\mathbf{b} - \mathbf{a}\wedge\mathbf{b}
$$

Both scalar and wedge product exist in any number of dimensions and are defined as:

$$
  \mathbf{a}\cdot\mathbf{b} := \frac{\mathbf{a}\mathbf{b}+\mathbf{b}\mathbf{a}}{2} \qquad \mathbf{a}\wedge\mathbf{b} := \frac{\mathbf{a}\mathbf{b}-\mathbf{b}\mathbf{a}}{2}
$$

The two products satisfy the following rules which can easily be interfered from our general rules:

$$
  \mathbf{e}_i \cdot \mathbf{e}_i = 1 \qquad \mathbf{e}_i \cdot \mathbf{e}_j = 0 \\[1ex]
  \mathbf{e}_i \wedge \mathbf{e}_i = 0 \qquad \mathbf{e}_i \wedge \mathbf{e}_j = -\mathbf{e}_j \wedge \mathbf{e}_i
$$

Since the wedge product can only increase the order of its input, this allows one to extract the higher part of vector products:

$$
  \mathbf{a}\wedge\mathbf{b} = (\mathbf{a}\times\mathbf{b}) I \\[1ex]
  \mathbf{a}\wedge\mathbf{b}\wedge\mathbf{c} = \det([\mathbf{a}|\mathbf{b}|\mathbf{c}]) I
$$

This even works in n dimensions as long as n vectors are multiplied for the determinant and n-1 vectors for the cross product - which then returns another pseudovector in a direction orthogonal to all of them.

The [Kronecker delta](https://en.wikipedia.org/wiki/Kronecker_delta) and the [Levi Civita symbol](https://en.wikipedia.org/wiki/Levi-Civita_symbol) are tensors with similar structure to the scalar and wedge products.

## Part 6: Matrix representations

It should also be noted that for every geometric algebra there exists a set of square matrices behaving like the basis vectors when combined with the usual matrix addition and multiplication.

In two dimensions one can identify:

$$
  \mathbf{e}_x = \begin{pmatrix}1 & 0\\ 0 & -1\end{pmatrix} \quad \mathbf{e}_y = \begin{pmatrix}0 & 1\\ 1 & 0\end{pmatrix}
$$

In three dimensions the ordinary basis vectors can be identified with the [Pauli matrices](https://en.wikipedia.org/wiki/Pauli_matrices):

$$
  \mathbf{e}_x = \begin{pmatrix}0 & 1\\ 1 & 0\end{pmatrix} \quad \mathbf{e}_y = \begin{pmatrix}0 & -i\\ i & 0\end{pmatrix} \quad \mathbf{e}_z = \begin{pmatrix}1 & 0\\ 0 & -1\end{pmatrix}
$$

In four dimensions we can also use the 2x2 Pauli matrices \\( \sigma_i \\) to find:

$$
  \mathbf{e}_t = \begin{pmatrix}I_2 & 0\\ 0 & -I_2\end{pmatrix} \quad \mathbf{e}_x = \begin{pmatrix}0 & \sigma_x\\ \sigma_x & 0\end{pmatrix} \quad \mathbf{e}_y = \begin{pmatrix}0 & \sigma_y\\ \sigma_y & 0\end{pmatrix} \quad \mathbf{e}_z = \begin{pmatrix}0 & \sigma_z\\ \sigma_z & 0\end{pmatrix}
$$

And the spacetime basis vectors correspond to the [gamma matrices](https://en.wikipedia.org/wiki/Gamma_matrices):

$$
  \bm{\gamma}_0 = \begin{pmatrix}I_2 & 0\\ 0 & -I_2\end{pmatrix} \quad \bm{\gamma}_1 = \begin{pmatrix}0 & \sigma_x\\ -\sigma_x & 0\end{pmatrix} \quad \bm{\gamma}_2 = \begin{pmatrix}0 & \sigma_y\\ -\sigma_y & 0\end{pmatrix} \quad \bm{\gamma}_3 = \begin{pmatrix}0 & \sigma_z\\ -\sigma_z & 0\end{pmatrix}
$$

One well-known [consequence](https://en.wikipedia.org/wiki/Complex_number#Matrix_representation_of_complex_numbers) of this is that any complex number can be written as:

$$
  a+bi = a\begin{pmatrix}1 & 0\\ 0 & 1\end{pmatrix} + b\begin{pmatrix}0 & 1\\ -1 & 0\end{pmatrix} = \begin{pmatrix}a & b\\ -b & a\end{pmatrix}
$$

Applying Euler's formula gives the fact that [rotation matrices](https://en.wikipedia.org/wiki/Rotation_matrix) can be expressed as matrix exponential:

$$
  \exp\begin{pmatrix}0 & \varphi\\ -\varphi & 0\end{pmatrix} = \begin{pmatrix}\cos(\varphi) & \sin(\varphi)\\ -\sin(\varphi) & \cos(\varphi)\end{pmatrix}
$$

Another interesting fact, the relationship beween [Pauli matrices and the scalar and cross product](https://en.wikipedia.org/wiki/Pauli_matrices#Relation_to_dot_and_cross_product), is immediately obvious when looking at it from a geometric algebra perspective:

$$
  (\mathbf{a}\cdot\bm{\sigma})(\mathbf{b}\cdot\bm{\sigma}) = (\mathbf{a}\cdot\mathbf{b}) + i(\mathbf{a}\times\mathbf{b})\cdot \bm{\sigma}
$$

Again a matrix representation of quaternions can be found using the products of the Pauli matrices:

$$
  a+bi+cj+dk = a\begin{pmatrix}1 & 0\\ 0 & 1\end{pmatrix} + b\begin{pmatrix}i & 0\\ 0 & -i\end{pmatrix} + c\begin{pmatrix}0 & 1\\ -1 & 0\end{pmatrix} + d\begin{pmatrix}0 & i\\ i & 0\end{pmatrix} = \begin{pmatrix}a+bi & c+di\\ -c+di & a-bi\end{pmatrix}
$$

## Summary

Geometric algebra elegantly unifies the concepts of scalar products, cross products, determinants, complex numbers and quaternions and allows us to generalize them to any number of dimensions.

Together with associativity and distributivity, the only rules needed to define a geometric algebra are:

$$
  \{\mathbf{e}_i,\mathbf{e}_j\} = 2\eta_{ij}
$$

Where \\( \eta_{ij} \\) is a metric tensor with [signature](https://en.wikipedia.org/wiki/Metric_signature) \\( (p, q) \\). Since \\( \eta_{ij} \\) is symmetric, there always exists a orthogonal set of basis vectors so that \\( \eta_{ij} \\) [is diagonal](https://en.wikipedia.org/wiki/Spectral_theorem), with p positive and q negative diagonal entries.
