+++
Description = "My workflow for deploying to GitHub pages"
date = "2018-06-07"
title = "One way to create animations of Keras models learning"
+++

Say you want to create an animation that shows how a neural network learns from data. Maybe something like this, where a neural network learns the shape of a sine function:

<iframe width="560" height="315" src="https://www.youtube.com/embed/jzYQYGXYPpA" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Here's how I did that. First, get the code for the neural network, so you can follow along: [ml-sine on GitHub](https://github.com/p886/ml-sine)

When training your model you'll usually use a statement like this:

```python
model.fit(x_train, y_train, batch_size=128, epochs=100)
```

After every epoch we want to take a snapshot of how well our prediction fits the training data. So the first step would be to iterate the epochs manually:

```python
for epoch in np.arange(1, epochs):
      self.model.fit(x_train, y_train, batch_size=128)
```

this allows us to run additional code in between the training epochs. The code we want to run between should save a plot of our prediction and the 'ground-truth' `y_train`. We'll use `matplotlib` to achieve this:

```python
import matplotlib.pyplot as plt

plt.plot(x_train, y_train, 'g.', ms=1)
plt.plot(x_train, self.model.predict(x_train), 'r.', ms=1)
plt.axis([
   x_train.min() - 0.5, x_train.max() + 0.5,
   y_train.min() - 0.5, y_train.max() + 0.5,
])
plt.title(f"Epoch #{epoch}")
plt.savefig(f"img/plot_{epoch}.png")
plt.clf()
```

We're using `plt.savefig()` to store an image of the plot on disk. We will end up with 100 images that we can turn into a slideshow video using `ffmpeg`. Assuming the images are stored in a subfolder named `img`:

```bash
ffmpeg -n -framerate 4  -i "img/plot_%d.png"  -vf "fps=25,format=yuv420p" movie.mp4
```

The option `-framerate 4` makes each image appear for a quarter of a second.
