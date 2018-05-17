# GovTechStack.sg Centos Docker Containers

This is a series of Makefile and Dockerfile scripts to build Centos 7 container images for use in GovTechStack.sg training sessions.

# Available images

These images are available from Dockerhub:

	govtechstacksg/centos:7
	govtechstacksg/haproxy:1.8.8
	govtechstacksg/nodejs:9.11.1
	govtechstacksg/python:2.7.15
	govtechstacksg/python:3.6.5
	govtechstacksg/redis:4.0.9
	govtechstacksg/ruby:2.5.1


# Using the Docker container image

To pull an image to your desktop, run the following commands:
```bash
docker pull govtechstacksg/centos:7
```

To run the image, run the following commands:
```bash
docker run -it --rm govtechstacksg/centos:7 bash
```
