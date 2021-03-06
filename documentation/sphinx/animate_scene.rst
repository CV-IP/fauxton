Example: Animation
==================
The following script reads a scene from :download:`a ".blend" file <scene.blend>`, shifts and rotates a prop within that scene, and displays the resulting video::

    #!/usr/bin/env python
    from os.path import dirname, join
    from matplotlib.pyplot import imshow
    from fauxton import Action, read_scene

    #===============================================================================
    # Rendering
    #===============================================================================

    scene_path = join(dirname(__file__), 'scene.blend')
    scene = read_scene(scene_path)

    scene['Camera'].resolution = (64, 64)
    scene['Cube'].action = Action(position=[(0, 0, 0, 0), (100, 0, 1, 1)],
                                  rotation=[(0, 0, 0, 0, 1), (100, 0, 1, 0, 0)])
    def render_frame(t):
        scene.time = t
	return scene['Camera'].render()

    #===============================================================================
    # Visualization
    #===============================================================================

    plot = imshow([[0]])
    plot.axes.set_title('Animation')
    plot.axes.set_axis_off()
    plot.figure.show()

    for t in range(100):
        plot.set_data(render_frame(t))
	plot.figure.canvas.draw()

**Output**:

.. image:: animate_scene.png
    :align: center
    :scale: 75%
