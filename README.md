Download Link: https://assignmentchef.com/product/solved-cs180-assignment4-simulate-the-modern-graphics-pipeline
<br>
In this assignment, we will move one step forward to simulate the modern graphics pipeline. We have updated our skeleton code by adding the object loader, vertex shader and fragment shader stage, and also support texture mapping. The task for you in this assignment is to interpolate the attributes of triangle (normals, colors, texture coordinates, etc.), and use them to implement the Blinn-Phong shading model in the phong fragment shader() function for only the shading part. When you get Blinn-Phong reflectance model implemented, you can move on to the textured shading fragment shader. When you are working on the textured shading, write your code into the texture fragment shader() function in main.cpp file. Then, you actually need to change the active shader of the rasterizer. You should do it this way: in the main.cpp file, there is a variable called active shader(). You can take a look at how we set it to different shaders in the code, and update it to your shader. Also, if you are using the shaders we have provided for you, you can use the command line interface to choose the shader that you want to use. The details of the command line interface is explained below in more detail. Functions that you need to modify:

<ul>

 <li>get projection matrix() in cpp: Use your implementation for the projection matrix from the previous assignments.</li>

 <li>phong fragment shader() in cpp: Compute the fragment color according to blinn-phong reflection model.</li>

 <li>texture fragment shader() in cpp: Compute the fragment color according to blinn-phong reflection model. Use the texture color as the kd coefficient in the formula.</li>

 <li>rasterize triangle(const Triangle&amp; t) in cpp: Interpolate the needed attributes, and pass them to the fragment shader payload.</li>

</ul>

Initially, the code is compilable, and you can run the program directly by saying ./Rasterizer, and it will initiate with an already implemented shader that visualizes the normals of the mesh, although since you will not have implemented the rasterize triangle() function, you will not see anything. When you implement the function, you should be able to see the normals of the shape. After you implement your own shaders phong fragment shader() and texture fragment shader(), you should look for a variable called active shader inside the main function. That variable sets the shader to be passed to the rasterizer. Go ahead and update this variable to your shader (or, you can use the command line tool to choose among already implemented shaders), and voila! You’re ready to run your cool shader.

<h1>1           Getting started</h1>

Same as the previous assignments, you can either choose to work in your own system, or to use the virtual machine. Download the skeleton code package for assignment 4. When building the project from command line, we require you do the following, create a folder named build under Assignment4 directory:

1 mkdir build 2 cd ./build 3 cmake ..

4 make

This will generate the executable name Rasterizer. There is a slightly extended command line tool that you can use with this executable. As usual, the second argument you give to the executable will be the output image name. The third argument can be:

<ul>

 <li>texture: Activates the <strong>texture </strong>shader in your code.</li>

</ul>

Example usage: ./Rasterizer output.png texture

<ul>

 <li>normal: Activates the <strong>normal </strong>shader in your code.</li>

</ul>

Example usage: ./Rasterizer output.png normal

<ul>

 <li>phong: Activates the <strong>blinn-phong </strong>shader in your code. Example usage: ./Rasterizer output.png phong</li>

</ul>

Whenever you make changes to the code and want to see the new result, you need to type make again.

We have made several changes to the framework:

<ul>

 <li>We have included a third party .obj files loader library to load more complex model from files. This exist in the OBJ Loader.h You don’t have to understand how this loader works in detail. Just be aware of that it generates a vector of triangles for us, which we call TriangleList in the program. Each triangle will also be assigned per vertex normals and texture coordinates. Meanwhile, texture-associated with this model will also be loaded. <strong>Note: If you want to load other objects, you have to change the path manually for now.</strong></li>

 <li>We have introduced a new texture class to create textures from images, and provided the interface to perform the texture color lookup function: Vector3f getColor(float u, float v).</li>

 <li>We created the hpp header file and defined the fragment shader payload, which contains the attributes that are will use by your fragment shaders. Currently there are three fragment shaders in the main.cpp. The fragment shader is the example shader that shade the fragment according to its normal vector. The other two are left for you to implement.</li>

 <li>The main rendering pipeline starts in rasterizer::draw(std::vector&lt;Triangle&gt; &amp;TriangleList).</li>

</ul>

We perform a series of transformations here. Usually it is the job for the vertex shader. Then we call the rasterize triangle function.

<ul>

 <li>The rasterize triangle function is similar to what you did in assignment 3. Instead of assigning constant value, you need to do interpolation for normals, colors, texture colors and shading colors using Barycentric Coordinates. Recall [alpha, beta, gamma] we provided last time to calculate the interpolated z value, now it is time for you to apply them to more other properties. What you need to do is to calculate interpolated color, and write the color computed by fragment shader into the framebuffer. This requires you to set up the fragment shader payload first using the interpolated attributes and to call the fragment shader to get result.</li>

</ul>

After you copy-pasted your code from the last assignment and make revision based on the instruction above (<strong>Please make sure that you read through what we have revised from last time</strong>). You can run the program with the default normal fragment shader, you will see this:

The shading result after your implementation of the Blinn-Phong reflectance model will look like:

Result after your implementation of the textured shading will look like:

Note that the skeleton code uses slightly different parameters, so some of the images you generated should not be identical to the images shown in here. You can try to adjust parameters on your own, for example, the light positions, the p value. But <strong>please make sure the rendered images you submit are using the parameters hard-coded in the skeleton code</strong>.