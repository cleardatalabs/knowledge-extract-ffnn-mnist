# Knowledge extraction from feed forward neural network trained on MNIST database

## A bit of background

Inspired by online demo of a neural net for handwritten digit recognition. 
http://myselph.de/neuralNet.html by Hubert Eichner.

The network was trained on the MNIST dataset in MATLAB and then trained model was exported to JavaScript.
In conjunction with drawing tool it makes possible to recognize user handwritten digits directly in browser.
According to author model achieves recognition error on the test data set is 1.92% (9808/10000 digits classified correctly),
which stands quite well even among other MNIST "competitors".  

Great result, awesome presentation. Is there anything to add? Let's try!

## Knowledge is power. - Francis Bacon

While trained model can produce the result, there's also lot of interest around *how* the result was achieved. 
People are looking for the explanation of "black box" like getting set of rules or input influences.
Various approaches exist for that purpose which differ by their complexity and neural network structural assumptions:  
https://www.eng.tau.ac.il/~michaelm/barca.pdf
https://www.researchgate.net/publication/3715258_Knowledge_extraction_from_artificial_neural_network_models

## Theory and Practice

Network has 784 input units (28 x 28 grayscale image, normalized to values ranging from [-1; 1]). 
and output layer with 10 units, assigning class (0 - 9) probabilities to the input image.
Structure and weight values are known from the sourcecode 
(network code itself extracted from original page and placed into separate file)
```
<script src="./net.js"></script>
``` 

So it's possible to calculate casual index as sum of the product of all “pathways” between each input to each output,
 

C<sub>i</sub> = &sum;<sub>j=0</sub><sup>h</sup> W<sub>kj</sub> * W<sub>ji</sub>

where there are h hidden neurons, 
Wkj are the connection weights from hidden neuron j to output k, 
Wji are the connection weights between input i to hidden neuron j.

In Javascript it would look like:
```
function getInfluence(inputIndex, outIndex) {
  var sum = 0;
  for (var i=0; i<w12.length; i++) {
    sum += w12[i][inputIndex]*w23[outIndex][i];
  }
  return sum;
}
```

What is left is to visualize the results by creating 10 input "heat maps" for each of the digit classes
and painting them according to calculated Ci values. This is done via 
```
function draw(canvasNumber, index, value)
```
which paints rectangle on canvas defined by *canvasNumber* on position corresponding to *index* in color proportional to the *value*

## Conclusion
As a result we've got 10 images explaining impact of each input pixel to the recognized class. Interesting fact is similarity between these impact hotmaps and original digit sampe images

![0](http://cleardatalabs.com/knowledge-extract-ffnn-mnist/char0.png)

![3](http://cleardatalabs.com/knowledge-extract-ffnn-mnist/char3.png)

Online demo page with all the digits and canvases painted at runtime available at: 
http://cleardatalabs.com/knowledge-extract-ffnn-mnist/ 

## Next steps
The influence (casual index) method used is pretty easy and fast one. For feed forward networks with known and simple structure it works pretty well. However more complex structures (or even "black box" models) might require different approaches i.e. adjusting the image to fixed network structure and output class like it was done in [Deep Dream](https://en.wikipedia.org/wiki/DeepDream). 
If you found it interesting kindly let us know by:
- asking a question via creating issue
- giving project a star on GitHub
- following us on [Twitter](https://twitter.com/ClearDataLabs)

To be continued http://cleardatalabs.com/knowledge-extract-ffnn-mnist/index2.html
