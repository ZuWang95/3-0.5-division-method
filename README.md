# A Subdivision Scheme in igl

![](img/sqrt.png?raw=true)

√3 Subdivision. From left to right: original mesh, added
vertices at the midpoints of the faces (step 1), connecting the new points
to the original mesh (step 1), flipping the original edges to obtain a new
set of faces (step 3). Step 2 involves shifting the original vertices and is
not shown.

Implementation of the subdivision scheme described in
[Kobbelt00sqrt(3)-subdivision](https://www.graphics.rwth-aachen.de/media/papers/sqrt31.pdf) to
iteratively create finer meshes from a given coarse mesh.
According to the paper, given a mesh `(V, F)`, the √3-subdivision scheme creates a new mesh `(V', F')`
by applying the following rules:

 1. Add a new vertex at location `m_f` for each face `f` in `F` of the original mesh.
    The new vertex will be located at the midpoint of the face.
    Append the newly created vertices `M = {m_f}` to `V` to create a new set of vertices `V'' = [V; M]`.
    Add three new faces for each face f in order by connecting `m_f` with edges to the original 3
    vertices of the face; we call the set of this newly created faces `F''`.
    <!-- Replace the old set of faces `F` with `F''`. -->
 2. Move each vertex `v` of the old vertices `V` to a new position `p` by averaging `v` with the positions of its neighboring vertices in the *original* mesh.
    If `v` has valence `n` and its neighbors in the original mesh `(V, F)` are `v_0`, `v_1`, ...,  `v_n`, then the update rule is<br/>
    ![](https://latex.codecogs.com/png.latex?\fg_gray%20p=(1-a_n)v&plus;\frac{a_n}{n}\sum\limits_{i=0}^{n-1}v_i)<br/>
    <!-- p = (1-a_n) v + \frac{a_n}{n}\sum\limits_{i=0}^{n-1} v_i -->where<br/>
    ![](https://latex.codecogs.com/png.latex?\fg_gray%20a_n=\frac{4-2\cos\left(\frac{2\pi}{n}\right)}{9}.)<br/>
    <!-- a_n=\frac{4-2\cos\left(\frac{2\pi}{n}\right)}{9} -->
    The vertex set of the subdivided mesh is then `V' = [P, M]`, where `P` is the concatenation of the new positions `p` for all vertices.
  3. Replace the `F''` with a new set of faces `F'` such that the edges connecting the newly added points `M` to `P`
    (the relocated original vertices) remain but the original edges of the mesh connecting points in `M` to each other are flipped.



![](img/subdivision.png?raw=true)


