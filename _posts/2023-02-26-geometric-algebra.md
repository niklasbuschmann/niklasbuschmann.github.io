---
title: "Introduction to Geometric Algebra"
date: 2023-02-26
description: An introduction to geometric algebra
mathjax: true
layout: post
---

In this post we will start in two dimensions and derive the scalar product and two-dimensional analog of the cross product, assuming only that they are invariant under rotations of the coordinate system. 

We will then derive a set of rules that define something named [geometric algebra](https://en.wikipedia.org/wiki/Geometric_algebra) which allows us to generalize the concept to any number of dimensions. A more detailed description of the resulting two-dimensional geometric algebra can be found in Joe Gregorio's [Introduction to Geometric Algebra over R^2](https://bitworking.org/news/ga/2d/).


### Table of contents

- [Vector Multiplication](#part-1-vector-multiplication)
- [Geometric Algebra](#part-2-geometric-algebra)
- [Three dimensions](#part-3-three-dimensions)
   - [Quaternions](#quaternions)
   - [Electromagnetism](#electromagnetism)
- [Four dimensions](#part-4-four-dimensions)
- [Scalar and wedge product](#part-5-scalar-and-wedge-product)
- [Matrix representations](#part-6-matrix-representations)
- [Summary](#summary)

## Part 1: Vector Multiplication

Starting in two dimensions, any vector \\( \mathbf{a} \\) can be written as combination of the two basis vectors \\( \mathbf{e_x} \\) and \\( \mathbf{e_y} \\):

$$
  \mathbf{a} = \begin{pmatrix}a_1\\a_2\end{pmatrix} = a_1\begin{pmatrix}1\\0\end{pmatrix} + a_2\begin{pmatrix}0\\1\end{pmatrix} =: a_1 \mathbf{e_x}+a_2 \mathbf{e_y}
$$

What is the product \\( \mathbf{ab} \\) of two vectors \\( \mathbf{a} \\) and \\( \mathbf{b} \\)? One fundamental property associated with multiplication is [distributivity](https://en.wikipedia.org/wiki/Distributive_property) over addition:

$$
  (\mathbf{a}+\mathbf{b})\mathbf{c} = \mathbf{a}\mathbf{c} + \mathbf{b}\mathbf{c} \qquad \mathbf{c}(\mathbf{a}+\mathbf{b})=\mathbf{c}\mathbf{a}+\mathbf{c}\mathbf{b}
$$

Assuming distributivity, the vector multiplication can be reduced to the multiplication of the basis vectors \\( \mathbf{e}\_i \\):

$$
\begin{aligned}
  \mathbf{a}\mathbf{b}
  &= (a_1 \mathbf{e_x}+a_2 \mathbf{e_y})(b_1 \mathbf{e_x}+b_2 \mathbf{e_y})\\[1ex]
  &= a_1b_1\mathbf{e_x}\mathbf{e_x}+a_1b_2\mathbf{e_x}\mathbf{e_y}+a_2b_1\mathbf{e_y}\mathbf{e_x}+a_2b_2\mathbf{e_y}\mathbf{e_y}
\end{aligned}
$$

Besides distributivity, the only assumption about the product is that for two vectors it should only depend on the relative angle between them, not on the absolute angles of the vectors themselves. In other words: **The product should stay invariant under rotations of the coordinate system.**

Let us now consider for example the product of a unit vector \\( \mathbf{\hat{u}} \\) with itself. We could choose \\( \mathbf{\hat{u}} \\) to point:
 - along the x-axis
 - somewhere between x and y-axis
 - along the y-axis

Calculating the three products and requiring them to be equal yields:

$$
  \mathbf{e_x}\mathbf{e_x} \overset{!}{=} u_1^2 \mathbf{e_x}\mathbf{e_x} + u_1u_2 \mathbf{e_x}\mathbf{e_y} + u_2u_1\mathbf{e_y}\mathbf{e_x} + u_2^2 \mathbf{e_y}\mathbf{e_y} \overset{!}{=} \mathbf{e_y}\mathbf{e_y}
$$

Since \\( u_1^2+u_2^2 = 1 \\) the two fundamental properties of the multiplication follow:

$$
  \mathbf{e_x}\mathbf{e_x} = \mathbf{e_y}\mathbf{e_y} \qquad \mathbf{e_x}\mathbf{e_y} = -\mathbf{e_y}\mathbf{e_x}
$$

The product can thus be simplified to:

$$
\begin{aligned}
  \mathbf{a}\mathbf{b}
  &= (a_1 b_1 + a_2 b_2)\mathbf{e_x}\mathbf{e_x} + (a_1b_2 - a_2b_1)\mathbf{e_x}\mathbf{e_y} \\[1ex]
  &= (\mathbf{a}\cdot\mathbf{b})\mathbf{e_x}\mathbf{e_x} + (\mathbf{a}\times\mathbf{b})\mathbf{e_x}\mathbf{e_y}
\end{aligned}
$$

The result contains only two components \\( \mathbf{e_x}\mathbf{e_x} \\) and \\( \mathbf{e_x}\mathbf{e_y} \\), which are called scalar and pseudoscalar, because the scalar stays the same when \\( \mathbf{e_x} \\) and \\( \mathbf{e_y} \\) are flipped and the pseudoscalar changes its sign. The scalar part resembles the definition of the [dot product](https://en.wikipedia.org/wiki/Dot_product) \\(\ \mathbf{a}\cdot\mathbf{b} = a_1b_1 + a_2b_2 \\) and the pseudoscalar part resembles the [cross product](https://en.wikipedia.org/wiki/Cross_product) \\(\ \mathbf{a}\times\mathbf{b} = a_1b_2 - a_2b_1 \\), since those two are the products invariant under rotations in the plane.

## Part 2: Geometric Algebra

We found two fundamental types of multiplication: one that stays the same when the vectors are switched and one that changes its sign.

If we additionally assume [associativity](https://en.wikipedia.org/wiki/Associative_property) of the product then we can see how \\( \mathbf{1} := \mathbf{e_x}\mathbf{e_x} \\) and \\( \bm{I} := \mathbf{e_x}\mathbf{e_y} \\) are multiplied with each other:

$$
  \mathbf{1}\cdot\mathbf{1} = \mathbf{e_x} \mathbf{e_x} \mathbf{e_y} \mathbf{e_y} = - \mathbf{e_x} \mathbf{e_y} \mathbf{e_x} \mathbf{e_y} = -\bm{I}\cdot\bm{I} \\[1ex]
  \quad \mathbf{1}\cdot\bm{I} = \mathbf{e_x} \mathbf{e_x} \mathbf{e_x} \mathbf{e_y} = -\mathbf{e_x} \mathbf{e_x} \mathbf{e_y} \mathbf{e_x} = \mathbf{e_x} \mathbf{e_y} \mathbf{e_x} \mathbf{e_x} = \bm{I}\cdot\mathbf{1}
$$

The product of an arbitrary combination of \\( \mathbf{1} \\) and \\( \bm{I} \\) can thus be written as:

$$
  (a_1\mathbf{1} + a_2\bm{I})(b_1\mathbf{1} + b_2\bm{I}) = (a_1b_1 - a_2b_2)\mathbf{1}\cdot\mathbf{1} + (a_1b_2 + a_2b_1)\mathbf{1}\cdot\bm{I}
$$

We can see that \\( \\{\mathbf{1}, \bm{I}\\} \\) becomes [isomorphic](https://en.wikipedia.org/wiki/Isomorphism) to the [complex numbers](https://en.wikipedia.org/wiki/Complex_number) \\( \\{1, i\\} \\) if we *define* \\( \mathbf{1} \\) to be the [neutral element](https://en.wikipedia.org/wiki/Identity_element) \\( 1 \\) of the multiplication.

By definition we know that vectors are left unchanged when multiplied with the neutral element \\( 1 \\). But how does \\( \bm{I} \\) act on the ordinary basis vectors?

$$
  \bm{I} \mathbf{e_x} = \mathbf{e_x} \mathbf{e_y} \mathbf{e_x} = - \mathbf{1}\mathbf{e_y} \\[1ex]
  \bm{I} \mathbf{e_y} = \mathbf{e_x} \mathbf{e_y} \mathbf{e_y} = + \mathbf{1}\mathbf{e_x}
$$

Because \\( \\{\mathbf{1}, \bm{I}\\} \\) is now closed under multiplication and both give back a vector when multiplied with a vector, the complete geometric algebra \\( \\{\mathbf{1}, \mathbf{e_x}, \mathbf{e_y}, \bm{I}\\} \\) is closed under multiplication, with the following multiplication table:

$$
  \mathbf{e_x}\mathbf{e_x} = \mathbf{1} \quad \mathbf{e_x}\mathbf{e_y} = \bm{I} \quad \mathbf{e_x} \bm{I} = \mathbf{e_y}\\[1ex]
  \mathbf{e_y}\mathbf{e_x} = - \bm{I} \quad \mathbf{e_y}\mathbf{e_y} = \mathbf{1} \quad \mathbf{e_y} \bm{I} = -\mathbf{e_x}\\[1ex]
  \bm{I}\mathbf{e_x} = -\mathbf{e_y} \quad \bm{I}\mathbf{e_y}=\mathbf{e_x} \quad \bm{I}^2 = -\mathbf{1}
$$

In fact the geometric algebra can be generalized to any number of dimensions where for any \\( i \neq j \\):

$$
  \mathbf{e}_i\mathbf{e}_i = \mathbf{1} \qquad \mathbf{e}_i\mathbf{e}_j = -\mathbf{e}_j\mathbf{e}_i
$$

Using the [anti-commutator](https://en.wikipedia.org/wiki/Commutator#Ring_theory) and the [delta symbol](https://en.wikipedia.org/wiki/Kronecker_delta) this can be written more elegantly as:

$$
  \frac{1}{2}\{\mathbf{e}_i,\mathbf{e}_j\} = \delta_{ij}
$$

## Part 3: Three Dimensions

In three dimensions there exist eight basis vectors:

$$
  \{\mathbf{1}, \mathbf{e_x}, \mathbf{e_y}, \mathbf{e_z}, \mathbf{e_y}\mathbf{e_z}, \mathbf{e_z}\mathbf{e_x}, \mathbf{e_x}\mathbf{e_y}, \mathbf{e_x}\mathbf{e_y}\mathbf{e_z} \}
$$

Again defining \\( \bm{I}:=\mathbf{e_x}\mathbf{e_y}\mathbf{e_z}\\) allows the basis to be rewritten as:

$$
  \{\mathbf{1}, \mathbf{e_x}, \mathbf{e_y}, \mathbf{e_z}, \bm{I}\mathbf{e_x}, \bm{I}\mathbf{e_y}, \bm{I}\mathbf{e_z}, \bm{I} \}
$$

Where we used that:

$$
  \bm{I}\mathbf{e_x} = \mathbf{e_y} \mathbf{e_z} \qquad
  \bm{I}\mathbf{e_y} = \mathbf{e_z} \mathbf{e_x} \qquad
  \bm{I}\mathbf{e_z} = \mathbf{e_x} \mathbf{e_y}
$$

Let us see what happens when two ordinary vectors are multiplied together:

$$
  \mathbf{a}\mathbf{b} = (a_1b_1 + a_2b_2+a_3b_3) + (a_1b_2-a_2b_1)\mathbf{e_x}\mathbf{e_y} - (a_1b_3-a_3b_1)\mathbf{e_z}\mathbf{e_x} + (a_2b_3-a_3b_2)\mathbf{e_y}\mathbf{e_z}
$$

Using \\( \bm{I} \\) this can again be written as:

$$
  \mathbf{a}\mathbf{b} = \mathbf{a}\cdot\mathbf{b} + (\mathbf{a}\times\mathbf{b}) \bm{I}
$$

The product of three vectors contains the [determinant](https://en.wikipedia.org/wiki/Determinant) or [triple product](https://en.wikipedia.org/wiki/Triple_product) in the pseudoscalar part:

$$
\begin{aligned}
  \mathbf{a}\mathbf{b}\mathbf{c}
  &= (\mathbf{a}\cdot\mathbf{b})\mathbf{c} + ((\mathbf{a}\times\mathbf{b})\cdot\mathbf{c}+((\mathbf{a}\times\mathbf{b})\times\mathbf{c}) \bm{I}) \bm{I}\\[1ex]
  &= (\mathbf{a}\cdot\mathbf{b})\mathbf{c} - (\mathbf{a}\cdot\mathbf{c})\mathbf{b} + (\mathbf{b}\cdot\mathbf{c})\mathbf{a} + \det([\mathbf{a}|\mathbf{b}|\mathbf{c}]) \bm{I}
\end{aligned}
$$

### Quaternions

Like in the two-dimensional case, the pseudovectors together with the scalar are closed under multiplication:

$$
  (\mathbf{e_x}\mathbf{e_y})^2 = (\mathbf{e_z}\mathbf{e_x})^2 = (\mathbf{e_y}\mathbf{e_z})^2 = -1 \\[2ex]
  (\mathbf{e_x}\mathbf{e_y})(\mathbf{e_z}\mathbf{e_x}) = \mathbf{e_y}\mathbf{e_z} \qquad (\mathbf{e_z}\mathbf{e_x})(\mathbf{e_y}\mathbf{e_z}) = \mathbf{e_x}\mathbf{e_y} \qquad (\mathbf{e_y}\mathbf{e_z})(\mathbf{e_x}\mathbf{e_y}) = \mathbf{e_z}\mathbf{e_x}
$$

Together \\( \\{\mathbf{1}, \mathbf{e_x}\mathbf{e_y}, \mathbf{e_z}\mathbf{e_x}, \mathbf{e_y}\mathbf{e_z}\\} \\) are isomorphic to the [quaternions](https://en.wikipedia.org/wiki/Quaternion) \\( \\{1, i, j, k\\} \\), and when multiplicated with ordinary vectors are quite useful as [three-dimensional rotors](https://en.wikibooks.org/wiki/Physics_Using_Geometric_Algebra/Basic_Geometric_Algebra#Rotation_(G3)).

### Electromagnetism

Another nice property of geometric algebra is that [Maxwell's equations](https://en.wikipedia.org/wiki/Maxwell%27s_equations) take on a particularly simple form when [expressed](https://en.wikipedia.org/wiki/Mathematical_descriptions_of_the_electromagnetic_field#Geometric_algebra_formulations) using it:

$$
  \left(\frac{1}{c}\frac{\partial}{\partial t} - \mathbf{\nabla}\right)\left(-\mathbf{E}+ c\mathbf{B}\bm{I}\right) = \frac{\rho}{\epsilon_0} + \mu_0c\mathbf{j}
$$

Expanding the product shows that the expression above is in fact equal to Maxwell's equations:

$$
  \underbrace{\left(\mathbf{\nabla}\cdot\mathbf{E}\right)}_\mathrm{Gauss} + 
  c\underbrace{\left(\mathbf{\nabla}\times\mathbf{B} - \frac{1}{c^2}\frac{\partial \mathbf{E}}{\partial t}\right)}_\mathrm{Ampere-Maxwell} + 
  \underbrace{\left(\mathbf{\nabla}\times\mathbf{E}+\frac{\partial \mathbf{B}}{\partial t}\right)}_\mathrm{Faraday} \bm{I} + 
  c\underbrace{\left(\mathbf{\nabla}\cdot\mathbf{B}\right)}_\mathrm{Gauss} \bm{I}
  = \frac{\rho}{\epsilon_0} + \mu_0c\mathbf{j}
$$

Using the [electric potential](https://en.wikipedia.org/wiki/Electric_potential) \\( \phi \\) and [magnetic potential](https://en.wikipedia.org/wiki/Magnetic_vector_potential) \\( \mathbf{A} \\) - and using the [Lorenz gauge](https://en.wikipedia.org/wiki/Lorenz_gauge_condition) - we can write:

$$
  \left(\frac{1}{c}\frac{\partial}{\partial t} + \mathbf{\nabla}\right)\left(\phi + c\mathbf{A}\right)
  = c\underbrace{\left(\frac{1}{c^2}\frac{\partial \phi}{\partial t}+\mathbf{\nabla} \cdot \mathbf{A}\right)}_{=\ 0} - \underbrace{\left(-\mathbf{\nabla}\phi - \frac{\partial \mathbf{A}}{\partial t}\right)}_{=\ \mathbf{E}} + c\underbrace{\left(\mathbf{\nabla} \times \mathbf{A}\right)}_{=\ \mathbf{B}}\bm{I} = -\mathbf{E}+ c\mathbf{B}\bm{I}
$$

This yields the known formular involving the [d'Alembert operator](https://en.wikipedia.org/wiki/D%27Alembert_operator):

$$
  \left(\frac{1}{c^2}\frac{\partial^2}{\partial t^2} - \nabla^2\right)\left(\phi + c\mathbf{A}\right) = \frac{\rho}{\epsilon_0} + \mu_0c\mathbf{j}
$$

<!---The Lorentz force law can then be expressed as:

$$
  (c\rho + \mathbf{j})(-\mathbf{E}+c\mathbf{B}\bm{I})
  = -\underbrace{(\mathbf{E}\cdot\mathbf{j})}_{\textrm{power}} - c\underbrace{(\rho\mathbf{E} + \mathbf{j}\times\mathbf{B})}_{\textrm{electric monopole force}} + c^2\underbrace{\left(\rho\mathbf{B} - \mathbf{j}\times\frac{\mathbf{E}}{c^2}\right)}_{\textrm{magnetic monopole force}} \bm{I} + c\underbrace{(\mathbf{B}\cdot\mathbf{j})}_{\textrm{power}}\bm{I}
$$
-->

## Part 4: Four Dimensions

In four dimensions there are now 16 basis vectors:

$$
  \{\mathbf{1}, \mathbf{e_t}, \mathbf{e_x}, \mathbf{e_y}, \mathbf{e_z}, \mathbf{e_t}\mathbf{e_x}, \mathbf{e_t}\mathbf{e_y}, \mathbf{e_t}\mathbf{e_z}, \mathbf{e_x}\mathbf{e_y}, \mathbf{e_z}\mathbf{e_x}, \mathbf{e_y}\mathbf{e_z}, \bm{I}\mathbf{e_t}, \bm{I}\mathbf{e_x}, \bm{I}\mathbf{e_y}, \bm{I}\mathbf{e_z}, \bm{I} \}
$$

Since there are now six bivector components, the product of two vectors does not again correspond to a four-component vector. Because of this, the cross product, generalized to four dimensions, would either return six components or require three vectors as input.

Working with four dimensions usually involves space-time, so I decided to name the added dimension \\( \mathbf{e_t}\\). And because spacetime is coupled to special relativity we will now switch to a different set of basis vectors:

$$
  \bm{\gamma}_0 := \mathbf{e_t} \qquad \bm{\gamma}_1 := \mathbf{e_t}\mathbf{e_x} \qquad \bm{\gamma}_2 := \mathbf{e_t}\mathbf{e_y} \qquad \bm{\gamma}_3 := \mathbf{e_t}\mathbf{e_z}
$$

One can verify that this again defines a geometric algebra - called [spacetime algebra](https://en.wikipedia.org/wiki/Spacetime_algebra) - where now \\( \bm{\gamma}_1, \bm{\gamma}_2, \bm{\gamma}_3\\) square to \\( -1 \\) instead of \\( 1 \\), requiring modified multiplication rules. Using the Minkowski metric \\( \eta\_{\mu\nu} \\) with signature \\( (+, -, -, -) \\) these new rules can be expressed as:

$$
  \frac{1}{2}\{\bm{\gamma}_\mu,\bm{\gamma}_\nu\} = \eta_{\mu\nu}
$$

One [consequence](https://en.wikipedia.org/wiki/Spacetime_algebra#Spacetime_split) of this is that when a spacetime vector is multiplied by \\( \bm{\gamma}_0 \\), the result behaves like an ordinary 3d vector plus a scalar equivalent to the time dimension.

Another interesting property is that in the spacetime algebra there exists a "square root" of the d'Alembert operator:

$$
  \mathbf{\nabla}^2 = \left(\bm{\gamma}_0 \frac{1}{c}\frac{\partial}{\partial t} + \bm{\gamma}_1 \frac{\partial}{\partial x} + \bm{\gamma}_2 \frac{\partial}{\partial y} + \bm{\gamma}_3 \frac{\partial}{\partial z} \right)^2 = \frac{1}{c^2}\frac{\partial^2}{\partial t^2} - \nabla^2
$$

Now, since the [octonions](https://en.wikipedia.org/wiki/Octonion) are not associative, there does not exist a subset of basis vectors isomorphic to them. But the complete three dimensional geometric algebra is isomorphic to the [biquaternions](https://en.wikipedia.org/wiki/Biquaternion) and in four dimensions the scalar, bivectors and pseudoscalar together are isomorphic to the [split-biquaternions](https://en.wikipedia.org/wiki/Split-biquaternion).

### Electromagnetism

Replacing \\( \\{\mathbf{1}, \mathbf{e_x}, \mathbf{e_y}, \mathbf{e_z} \\} \\) with \\( \\{\bm{\gamma}\_0, \bm{\gamma}\_1, \bm{\gamma}\_2, \bm{\gamma}\_3 \\} \\) cleans up the equations considerably. Using the usual definitions of the [four-gradient](https://en.wikipedia.org/wiki/Four-vector#Four-gradient) \\( \mathbf{\nabla} = \partial^\mu \bm{\gamma}\_\mu \\) and [four-current](https://en.wikipedia.org/wiki/Four-vector#Electromagnetism) \\( \mathbf{j} = j^\mu \bm{\gamma}\_\mu \\) yields:

$$
   \mathbf{\nabla}\mathbf{F} = \mathbf{\nabla}\mathbf{\nabla}\mathbf{A} = \mu_0\mathbf{j}
$$

Here \\( \mathbf{F} = F^{\mu\nu}\bm{\gamma}\_\mu\bm{\gamma}\_\nu \\) is a bivector of the spacetime algebra, whose components \\( F^{\mu\nu} \\) will be the \\( \mathbf{E} \\) and \\( \mathbf{B} \\) field.

## Part 5: Scalar and Wedge product

In three dimensions the geometric prouct can be written as:

$$
  \mathbf{a}\mathbf{b} = \mathbf{a}\cdot\mathbf{b} + (\mathbf{a}\times\mathbf{b}) \bm{I} \quad \Leftrightarrow \quad \mathbf{b}\mathbf{a} = \mathbf{a}\cdot\mathbf{b} - (\mathbf{a}\times\mathbf{b}) \bm{I}
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
  \mathbf{e_i} \cdot \mathbf{e_i} = 1 \qquad \mathbf{e_i} \cdot \mathbf{e_j} = 0 \\[1ex]
  \mathbf{e_i} \wedge \mathbf{e_i} = 0 \qquad \mathbf{e_i} \wedge \mathbf{e_j} = -\mathbf{e_j} \wedge \mathbf{e_i}
$$

Since the wedge product can only increase the order of its input, this allows one to extract the higher part of vector products:

$$
  \mathbf{a}\wedge\mathbf{b} = (\mathbf{a}\times\mathbf{b}) \bm{I} \\[1ex]
  \mathbf{a}\wedge\mathbf{b}\wedge\mathbf{c} = \det([\mathbf{a}|\mathbf{b}|\mathbf{c}]) \bm{I}
$$

This even works in n dimensions as long as n vectors are multiplied for the determinant and n-1 vectors for the cross product - which then returns another pseudovector in a direction orthogonal to all of them.

## Part 6: Matrix representations

It should also be noted that for every geometric algebra there exists a set of square matrices behaving like the basis vectors when combined with the usual matrix addition and multiplication.

In **two dimensions** one can identify:

$$
  \mathbf{e_x} = \begin{pmatrix}1 & 0\\ 0 & -1\end{pmatrix} \quad \mathbf{e_y} = \begin{pmatrix}0 & 1\\ 1 & 0\end{pmatrix}
$$

One well-known [consequence](https://en.wikipedia.org/wiki/Complex_number#Matrix_representation_of_complex_numbers) of this is that any complex number can be written as:

$$
  a+bi = a\begin{pmatrix}1 & 0\\ 0 & 1\end{pmatrix} + b\begin{pmatrix}1 & 0\\ 0 & -1\end{pmatrix}\begin{pmatrix}0 & 1\\ 1 & 0\end{pmatrix} = \begin{pmatrix}a & b\\ -b & a\end{pmatrix}
$$

Applying [Euler's formula](https://en.wikipedia.org/wiki/Euler%27s_formula) gives the fact that [rotation matrices](https://en.wikipedia.org/wiki/Rotation_matrix) can be expressed as matrix exponential:

$$
  e^{i\varphi} = \cos \varphi + i\sin \varphi \quad \Leftrightarrow \quad \exp\begin{pmatrix}0 & \varphi\\ -\varphi & 0\end{pmatrix} = \begin{pmatrix}\cos \varphi & \sin \varphi\\ -\sin \varphi & \cos \varphi \end{pmatrix}
$$

In **three dimensions** the ordinary basis vectors can be identified with the [Pauli matrices](https://en.wikipedia.org/wiki/Pauli_matrices):

$$
  \mathbf{e_x} = \begin{pmatrix}0 & 1\\ 1 & 0\end{pmatrix} \quad \mathbf{e_y} = \begin{pmatrix}0 & -i\\ i & 0\end{pmatrix} \quad \mathbf{e_z} = \begin{pmatrix}1 & 0\\ 0 & -1\end{pmatrix}
$$

The relationship beween [Pauli matrices and the scalar and cross product](https://en.wikipedia.org/wiki/Pauli_matrices#Relation_to_dot_and_cross_product), is immediately obvious when looking at it from a geometric algebra perspective:

$$
  (\mathbf{a}\cdot\bm{\sigma})(\mathbf{b}\cdot\bm{\sigma}) = (\mathbf{a}\cdot\mathbf{b}) + i(\mathbf{a}\times\mathbf{b})\cdot \bm{\sigma}
$$

A matrix representation of quaternions can be found using the products of the Pauli matrices:

$$
  a+bi+cj+dk = a\begin{pmatrix}1 & 0\\ 0 & 1\end{pmatrix} + b\begin{pmatrix}i & 0\\ 0 & -i\end{pmatrix} + c\begin{pmatrix}0 & 1\\ -1 & 0\end{pmatrix} + d\begin{pmatrix}0 & i\\ i & 0\end{pmatrix} = \begin{pmatrix}a+bi & c+di\\ -c+di & a-bi\end{pmatrix}
$$

In **four dimensions** we can compose the 2x2 Pauli matrices \\( \bm{\sigma}_i \\) to find:

$$
  \mathbf{e_t} = \begin{pmatrix}0 & \mathbf{1}\\ \mathbf{1} & 0\end{pmatrix} \quad \mathbf{e_x} = \begin{pmatrix}-\bm{\sigma}_x & 0\\ 0 & \bm{\sigma}_x\end{pmatrix} \quad \mathbf{e_y} = \begin{pmatrix}-\bm{\sigma}_y & 0\\ 0 & \bm{\sigma}_y\end{pmatrix} \quad \mathbf{e_z} = \begin{pmatrix}-\bm{\sigma}_z & 0\\ 0 & \bm{\sigma}_z\end{pmatrix}
$$

And the spacetime basis vectors \\( \bm{\gamma}_i \\) correspond to the [gamma matrices](https://en.wikipedia.org/wiki/Gamma_matrices):

$$
  \bm{\gamma}_0 = \begin{pmatrix}0 & \mathbf{1}\\ \mathbf{1} & 0\end{pmatrix} \quad \bm{\gamma}_1 = \begin{pmatrix}0 & \bm{\sigma}_x\\ -\bm{\sigma}_x & 0\end{pmatrix} \quad \bm{\gamma}_2 = \begin{pmatrix}0 & \bm{\sigma}_y\\ -\bm{\sigma}_y & 0\end{pmatrix} \quad \bm{\gamma}_3 = \begin{pmatrix}0 & \bm{\sigma}_z\\ -\bm{\sigma}_z & 0\end{pmatrix}
$$

This construction can be continued for **higher dimensions**, starting with an even dimension \\( n \\) then:

$$
  \mathbf{e}_i' = \begin{pmatrix}-\mathbf{e}_i & 0\\ 0 & \mathbf{e}_i\end{pmatrix} \qquad \mathbf{e}_{n+1}' = \begin{pmatrix}0 & \mathbf{1}\\ \mathbf{1} & 0\end{pmatrix} \qquad \mathbf{e}_{n+2}' = \begin{pmatrix}0 & -i\cdot\mathbf{1}\\ i\cdot\mathbf{1} & 0\end{pmatrix}
$$

## Summary

Geometric algebra elegantly unifies the concepts of scalar products, cross products, determinants, complex numbers and quaternions and allows us to generalize them to any number of dimensions.

Together with associativity and distributivity, the only rules needed to define a geometric algebra are:

$$
  \frac{1}{2}\{\mathbf{e}_i,\mathbf{e}_j\} = \eta_{ij}
$$

Where \\( \eta_{ij} \\) is a metric tensor with [signature](https://en.wikipedia.org/wiki/Metric_signature) \\( (p, q) \\). Since \\( \eta_{ij} \\) is symmetric, there always exists a orthogonal set of basis vectors so that \\( \eta_{ij} \\) [is diagonal](https://en.wikipedia.org/wiki/Spectral_theorem), with p positive and q negative diagonal entries.
