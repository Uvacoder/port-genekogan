---
layout: default
title: Works
description: A list of projects by Gene Kogan
redirect_from: /work.html
---

<div id="works">
{% include project.md title="Abraham" %}
{% include project.md title="ml4a" %}
{% include project.md title="Futurium installation" %}
{% include project.md title="Neural synthesis" %}
{% include project.md title="pix2pix webcam (meat puppet)" %}
{% include project.md title="Doodle Tunes" %}
{% include project.md title="WikiArt GAN" %}
{% include project.md title="Deepdream infinite loops" %}
{% include project.md title="Invisible Cities" %}
{% include project.md title="Deepdream Densecap" %}
{% include project.md title="What neural networks see" %}
{% include project.md title="alt-AI" %}
{% include project.md title="Cubist Mirror" %}
{% include project.md title="A Book from the Sky 天书" %}
{% include project.md title="Why is a raven like a writing desk?" %}
{% include project.md title="Experiments with style transfer" %}
{% include project.md title="Opera Toolkit" %}
{% include project.md title="Action coding" %}
{% include project.md title="Machine Yearning" %}
{% include project.md title="Ecohacker-Build" %}
{% include project.md title="Birl" %}
{% include project.md title="Kinect Projector Toolkit" %}
{% include project.md title="Acoustic portraits" %}
{% include project.md title="Listening to the ocean" %}
{% include project.md title="Color of words" %}
</div>


{% comment %}
{% include project.md title="A Book from the Sky 天书" %}
{% include project.md title="A Book from the Sky 天书" %}
{% include project.md title="A day in New York" %}
{% include project.md title="Accordions" %}
{% include project.md title="Acoustic portraits" %}
{% include project.md title="Along the Karakoram" %}
{% include project.md title="alt-AI" %}
{% include project.md title="Atlas Densecap" %}
{% include project.md title="Spectrogram sculptures" %}
{% include project.md title="Bezier ribbons" %}
{% include project.md title="Birl" %}
{% include project.md title="Bohemian Rhapsody t-SNE" %}
{% include project.md title="Branching" %}
{% include project.md title="Color of words" %}
{% include project.md title="Cubist Mirror" %}
{% include project.md title="Deepdream Densecap" %}
{% include project.md title="Deepdream prototypes" %}
{% include project.md title="Ecohacker-Build" %}
{% include project.md title="Flocking" %}
{% include project.md title="Gestural instruments" %}
{% include project.md title="Interference" %}
{% include project.md title="Invisible Cities" %}
{% include project.md title="Jaaga workshop" %}
{% include project.md title="Kaleidoscopes" %}
{% include project.md title="Kinect Projector Toolkit" %}
{% include project.md title="Learning to generate text and audio" %}
{% include project.md title="Listening to the ocean" %}
{% include project.md title="Lost in translation" %}
{% include project.md title="Machine Yearning" %}
{% include project.md title="Manta" %}
{% include project.md title="Meditation" %}
{% include project.md title="ml4a" %}
{% include project.md title="Mosaic" %}
{% include project.md title="ofxLearn" %}
{% include project.md title="Pop chemistry" %}
{% include project.md title="Remixing an HP inkjet printer" %}
{% include project.md title="Processing shader examples" %}
{% include project.md title="Robot Shakespeare" %}
{% include project.md title="Scratch ML visualization" %}
{% include project.md title="Scripting photoshop" %}
{% include project.md title="Sonic self-portrait" %}
{% include project.md title="Stars" %}
{% include project.md title="Style Transfer" %}
{% include project.md title="Tennis visualization" %}
{% include project.md title="The Rub" %}
{% include project.md title="Tidy photography" %}
{% include project.md title="Timelapse cubism" %}
{% include project.md title="Tip of the Tongue" %}
{% include project.md title="Tree" %}
{% include project.md title="Urban incantations" %}
{% include project.md title="Vertex rivers" %}
{% include project.md title="Xinjiang-Hunza compilation" %}

iterate:
{% for page in site.works %}
    % include project.md title="{{page.title}}" % <br/>
{% endfor %}

missing:
{% include project.md title="Urban incantations" %}
{% include project.md title="Along the Karakoram" %}
{% include project.md title="Xinjiang-Hunza compilation" %}
{% include project.md title="Bohemian Rhapsody t-SNE" %}
{% include project.md title="Jaaga workshop" %}
{% include project.md title="Scratch ML visualization" %}

{% endcomment %}
