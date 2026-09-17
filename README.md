# Face Morphing

I morph my face with the face of a celebrity, compute a mean over the population of faces
and use the population mean to create a caricature of myself.

**Everything is in [`main.ipynb`](main.ipynb).** There is a `requirements.txt` for all the
libraries we need. I have saved the results of the tasks into the folder called `output`.

## Defining correspondences

I first selected 2 images, one of me and one of a celebrity to be morphed, then I had to
crop the images so that they will look similar. Before we can morph two faces together, we
need to define corresponding points on both of them.

Now it is time to provide a triangulation of these points to be used for morphing. I used
the Delaunay triangulation, which does not produce triangles with very small angles.

## Computing the midway face

To compute the midway face we need to do three things:

1. Compute the average shape of the two faces.
2. Warp both faces into the average shape.
3. Average the colors of the triangles together.

To warp the faces into the average shape, we need to compute the affine transformation
that maps the average shape to the original shape. We have to do this for every triangle
that we got using Delaunay triangulation. Solving for a, b, c, d, e, f yields our 3x3
homogeneous transformation matrix.

We have to decide which pixel color to use if we inverse transform our pixel to fill and
get a location that is between pixels in the original image. I used nearest neighbor
interpolation.

## The morph sequence

For the morph sequence we are writing the following function:

```python
morphed_im = morph(im1, im2, im1_pts, im2_pts, tri, warp_frac, dissolve_frac)
```

I chose a frame rate of 30 frames per second and created 45 frames between 0 and 1.

## The "mean" face of the population

Here we want to compute the average face of a population of faces. For this I used a face
dataset of 37 Danish people. Since the facial keypoints were already annotated, I could
use them directly.

I have also warped my face into the shape of the average Danish face and warped the
average Danish face into my face shape, and got a somewhat disturbing result. (I actually
look like a friend of mine ahaha.)

## Caricatures: extrapolating from the mean

We can produce a caricature of my face by extrapolating from the mean face. To do this we
need to choose alphas that are outside of our [0,1] range.

## Gender change

Here I changed the gender of my face. I'm showing the morphing of just the shape, just the
appearance and both. Shape only is done by only warping and not dissolving the images;
appearance only is done by only dissolving and not warping.
