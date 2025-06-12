---
layout: post
title: Computer Graphics
subtitle: understanding how render works
excerpt_image: ../assets/images/img/computer_graphics_001.png
categories: "university"
tags: ["DirectX", "C++", "CG"]
top: 6
---

## Summary
In this course, I did six assignments.
1. Just draw circle made of lots of triangle
2. read and rotating bunny model with VBO and  matrix
3. rotate and zoom bunny model with mouse input(camera view port)
4. lighting and phong shading
5. add texture(diffuse and bump map)
6. add shadow(multi-sampling for soft shadow)

I upload result video and sixth real code.

---

## Language and Software
C++, visual studio

---

## Result video
https://youtu.be/G3IzZyQKpiU
> <iframe width="750" height="505" src="https://www.youtube.com/embed/G3IzZyQKpiU?si=pen9_sy7-59h9S91" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

---

## Sixth shadow code(except shader code)
```
    #include "stdafx.h"
    #include <stdlib.h>
    #include<GL/glew.h>
    #include <GLFW/glfw3.h>
    #include<glm/glm.hpp>
    #include<vector>
    #include"toys.h"
    #include<math.h>

    #define GLM_ENABLE_EXPERIMENTAL
    #define STB_IMAGE_IMPLEMENTATION
    #include<glm/gtc/type_ptr.hpp>
    #include<glm/gtx/transform.hpp>
    #include"j3a.hpp"
    #include"stb_image.h"
    using namespace glm;

    bool buttonPressed = false;
    double lastX, lastY;
    vec3 cameraPos = vec3(0, 0, 5);
    vec3 lightPosition = vec3(5, 5, 5);
    float theta = 0, phi = 0, fov = 60;

    void render(GLFWwindow* window);
    void init(void);
    void cursorPosCB(GLFWwindow* window, double x, double y) {
    	if(buttonPressed == true){
    		int width, height;
    		glfwGetWindowSize(window, &width, &height);
    		float dx = (x - lastX)/width;
    		float dy = (y - lastY)/height;
		
		theta += -dx*3.141592f;
		phi   += -dy * 3.141592f;
		if (phi > 1.4) phi = 1.4f;
		if (phi < -1.4)phi = -1.4f;
		cameraPos = vec3(
			rotate(theta, vec3(0, 1, 0))
			*rotate(phi, vec3(1,0,0))
			*vec4(0, 0, 5, 1)
		);
		lastX = x;
		lastY = y;
	}
    }
    void mouseButtonCB(GLFWwindow* window, int button, int state, int mods) {
    	if (button == GLFW_MOUSE_BUTTON_1)
    	{
    		if (state == GLFW_PRESS) {
    			buttonPressed = true;
    			glfwGetCursorPos(window, &lastX, &lastY);
    		}
    		else
    			buttonPressed = false;
    	}
    }

    void scrollCB(GLFWwindow* window, double, double y) {
    	fov += y*10;
    	if (fov > 170)fov = 170;
    	if (fov < 10)fov = 10;
    }

    int main(void) {
    	if (!glfwInit())										
    		exit(EXIT_FAILURE);
    #ifdef __APPLE__	
    	GLFWwindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);			
    	GLFWwindowHint(GLFW_CONTEXT_VERSION_MINOR, 1);
    	GLFWwindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
    	GLFWwindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    #endif
    	glfwWindowHint(GLFW_SAMPLES, 4);		
	
	GLFWwindow* window = glfwCreateWindow(640, 640, "Hello", NULL, NULL);
	glfwSetCursorPosCallback(window, cursorPosCB);						
																		
	glfwSetMouseButtonCallback(window, mouseButtonCB);
	glfwSetScrollCallback(window, scrollCB);
	glfwMakeContextCurrent(window);										
	
	glewInit();
	init();
	glfwSwapInterval(1);
	while (!glfwWindowShouldClose(window)) {							
		render(window);													
		glfwPollEvents();														
	}
	glfwDestroyWindow(window);
	glfwTerminate();
    }
    GLuint vertexBuffer = 0;
    GLuint normalBuffer = 0;
    GLuint texCoordBuffer = 0;
    GLuint vertexArrayID = 0;	
    GLuint elementArray = 0;
    GLuint textureID = 0;
    GLuint bumpID = 0;
    
    Program prog;
    Program shadowProg;
    
    GLuint shadowTex = 0;
    GLuint shadowDepth = 0;
    GLuint shadowFBO = 0;

    void init(void) {

	prog.loadShaders("shader.vert", "shader.frag");
	shadowProg.loadShaders("shadow.vert", "shadow.frag");
    #pragma region Init_rotateBunny
    	loadJ3A("dwarf.j3a");

	glGenVertexArrays(1, &vertexArrayID);
	glBindVertexArray(vertexArrayID);


	
	glGenBuffers(1, &vertexBuffer);
	glBindBuffer(GL_ARRAY_BUFFER, vertexBuffer);
	glBufferData(GL_ARRAY_BUFFER, nVertices[0] * sizeof(glm::vec3), vertices[0], GL_STATIC_DRAW);

	glGenBuffers(1, &normalBuffer);
	glBindBuffer(GL_ARRAY_BUFFER, normalBuffer);
	glBufferData(GL_ARRAY_BUFFER, nVertices[0] * sizeof(glm::vec3), normals[0], GL_STATIC_DRAW);

	glGenBuffers(1, &texCoordBuffer);
	glBindBuffer(GL_ARRAY_BUFFER, texCoordBuffer);
	glBufferData(GL_ARRAY_BUFFER, nVertices[0] * sizeof(glm::vec2), texCoords[0], GL_STATIC_DRAW);

	
	glBindBuffer(GL_ARRAY_BUFFER, vertexBuffer);
	glEnableVertexAttribArray(0);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 0, 0);

	glBindBuffer(GL_ARRAY_BUFFER, normalBuffer);
	glEnableVertexAttribArray(1);
	glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 0, 0);

	glBindBuffer(GL_ARRAY_BUFFER, texCoordBuffer);
	glEnableVertexAttribArray(2);
	glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, 0, 0);

	
	glGenBuffers(1, &vertexArrayID);
	glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, vertexArrayID);
	glBufferData(GL_ELEMENT_ARRAY_BUFFER, nTriangles[0] * sizeof(glm::u32vec3), triangles[0], GL_STATIC_DRAW);

	int w = 0, h = 0, n = 0;
	void* buf = stbi_load("dwarfD.jpg", &w, &h, &n, 4); // n must be 4
	
	//Generate texture & set parameters
	glGenTextures(1, &textureID);
	glBindTexture(GL_TEXTURE_2D, textureID);										
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);	
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
	//Send data & delete buffer
	glTexImage2D(GL_TEXTURE_2D, 0, GL_SRGB8_ALPHA8, w, h, 0, GL_RGBA, GL_UNSIGNED_BYTE, buf);

	glGenerateMipmap(GL_TEXTURE_2D);
	stbi_image_free(buf);

	buf = stbi_load("dwarfB.jpg", &w, &h, &n, 4); 
	//Generate texture & set parameters
	glGenTextures(1, &bumpID);
	glBindTexture(GL_TEXTURE_2D, bumpID);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
	//Send data & delete buffer
	glTexImage2D(GL_TEXTURE_2D, 0, GL_RGBA, w, h, 0, GL_RGBA, GL_UNSIGNED_BYTE, buf);
	glGenerateMipmap(GL_TEXTURE_2D);
	stbi_image_free(buf);

	//Shadow Map
	glGenTextures(1, &shadowTex);
	glBindTexture(GL_TEXTURE_2D, shadowTex);
	glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB32F, 1024, 1024, 0, GL_RGB, GL_FLOAT, 0);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);

	//Depth Map
	glGenTextures(1, &shadowDepth);
	glBindTexture(GL_TEXTURE_2D, shadowDepth);
	glTexImage2D(GL_TEXTURE_2D, 0, GL_DEPTH_COMPONENT32F, 1024, 1024, 0, GL_DEPTH_COMPONENT, GL_FLOAT, 0);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);
	glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);

	//Frame buffer object for Depth&Shadow Map
	glGenFramebuffers(1, &shadowFBO);
	glBindFramebuffer(GL_FRAMEBUFFER, shadowFBO);
	glFramebufferTexture(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, shadowTex, 0);
	glFramebufferTexture(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT, shadowDepth, 0);
	GLenum drawBuffers[] = { GL_COLOR_ATTACHMENT0 };
	glDrawBuffers(1, drawBuffers);
	if (glCheckFramebufferStatus(GL_FRAMEBUFFER) != GL_FRAMEBUFFER_COMPLETE) printf("FBO Error\n");
	glBindFramebuffer(GL_FRAMEBUFFER, GL_NONE);

    }

    void render(GLFWwindow* window) {
    	int width, height;
    	glfwGetFramebufferSize(window, &width, &height);
    	glEnable(GL_DEPTH_TEST);
	
	
	glBindFramebuffer(GL_FRAMEBUFFER, shadowFBO);
	glViewport(0, 0, 1024, 1024);
	glClearColor(1, 1, 1, 0);
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT); 

	glUseProgram(shadowProg.programID);
	mat4 shadowProjMat = ortho(-2.f, 2.f, -2.f, 2.f, 0.01f, 10.f);
	mat4 shadowViewMat = lookAt(lightPosition, vec3(0,0,0), vec3(0,1,0) );
	mat4 modelMat = mat4(1);//rotate(-theta, vec3(0, 1, 0))*rotate(-phi, vec3(1, 0, 0));
	mat4 shadowMVP = shadowProjMat * shadowViewMat *modelMat;

	GLuint loc = glGetUniformLocation(shadowProg.programID, "shadowMVP");
	glUniformMatrix4fv(loc, 1, false, value_ptr(shadowMVP));

	mat4 biasMatrix = translate(vec3(0.5))*scale(vec3(0.5));
	mat4 shadowBiasMVP = biasMatrix * shadowMVP;

	glBindVertexArray(vertexArrayID);
	glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, vertexArrayID);
	glDrawElements(GL_TRIANGLES, nTriangles[0] * 3, GL_UNSIGNED_INT, 0);

	glBindFramebuffer(GL_FRAMEBUFFER, GL_NONE);
	glViewport(0, 0, width, height);
	glClearColor(0, 0, 0, 0);
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT); 
	
	glUseProgram(prog.programID);

	mat4 rotMat = rotate(0 * 3.14159f / 180, glm::vec3(1, 0, 0)); 		
    #pragma region LinkMatrix
    	loc = glGetUniformLocation(prog.programID, "rotMT");
    	glUniformMatrix4fv(loc, 1, false, value_ptr(rotMat));
    #pragma endregion
	loc = glGetUniformLocation(prog.programID, "cameraPosition");
	glUniform3fv(loc, 1, value_ptr(cameraPos));

	mat4 viewingMat = lookAt(cameraPos, vec3(0), vec3(0, 1, 0));
	loc = glGetUniformLocation(prog.programID, "viewMat");
	glUniformMatrix4fv(loc, 1, false, value_ptr(viewingMat));

	mat4 projMat = perspective(fov*3.141592f / 180, width / (float)height, 0.1f, 100.0f);
	loc = glGetUniformLocation(prog.programID, "projMat");
	glUniformMatrix4fv(loc, 1, false, value_ptr(projMat));
	
	loc = glGetUniformLocation(prog.programID, "lightPos");
	glUniform3fv(loc, 1, value_ptr(lightPosition));

	glActiveTexture(GL_TEXTURE0); 
	glBindTexture(GL_TEXTURE_2D, textureID);
	loc = glGetUniformLocation(prog.programID, "diffTex");
	glUniform1i(loc, 0);

	glActiveTexture(GL_TEXTURE1); 
	glBindTexture(GL_TEXTURE_2D, bumpID);
	loc = glGetUniformLocation(prog.programID, "bumpTex");
	glUniform1i(loc, 1);

	loc = glGetUniformLocation(prog.programID, "shadowMVP");
	glUniformMatrix4fv(loc, 1, false, value_ptr(shadowMVP));
	
	glActiveTexture(GL_TEXTURE2); 
	glBindTexture(GL_TEXTURE_2D, shadowTex);
	loc = glGetUniformLocation(prog.programID, "shadowMap");
	glUniform1i(loc, 2);

	//Draw!
	glBindVertexArray(vertexArrayID);					
	glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, vertexArrayID);
	glDrawElements(GL_TRIANGLES, nTriangles[0] * 3, GL_UNSIGNED_INT, 0);

	glfwSwapBuffers(window);				
    }
```
