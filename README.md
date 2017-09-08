# KinectV2_PCL

### Environment & Device

the following is my environment & Device

***
*   Windows 10
*   VS2013
*   PCL 1.8
*   ckame 3.9
*   Kinect V2

***

**USB 3.0 is needed for the Kinect v2*

### Install & SetUp


Download PCL AllInOne pkg from http://unanancyowen.com/en/pcl18/

Download cmake from https://cmake.org/

Download Kinect SDK 2.0 for Kinect V2 from https://www.microsoft.com/en-us/download/details.aspx?id=44561

After installation, now we try to build a new PCL Project

create a new folder and we will create two new files in it later


	cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

	project(cloud_viewer)

	find_package(PCL 1.2 REQUIRED)

	include_directories(${PCL_INCLUDE_DIRS})
	link_directories(${PCL_LIBRARY_DIRS})
	add_definitions(${PCL_DEFINITIONS})

	add_executable (cloud_viewer cloud_viewer.cpp)
	target_link_libraries (cloud_viewer ${PCL_LIBRARIES})
	
create a new file called `CMakeLists.txt` , copy and paste it

	#include <iostream>
	#include <pcl/io/io.h>
	#include <pcl/io/pcd_io.h>
	#include <pcl/visualization/cloud_viewer.h>

	// typedef pcl::PointXYZ PointT;
	typedef pcl::PointXYZRGBA PointT;

	void viewerOneOff (pcl::visualization::PCLVisualizer& viewer)
	{
		// set background to black (R = 0, G = 0, B = 0)
		viewer.setBackgroundColor (0, 0, 0);
	}

	void viewerPsycho (pcl::visualization::PCLVisualizer& viewer)
	{
		// you can add something here, ex:  add text in viewer
	}

	int main (int argc, char *argv[])
	{
		pcl::PointCloud<PointT>::Ptr cloud (new pcl::PointCloud<PointT>);

		// Load .pcd file from argv[1]
		int ret = pcl::io::loadPCDFile (argv[1], *cloud);
		if (ret < 0) {
			PCL_ERROR("Couldn't read file %s\n", argv[1]);
			return -1;
		}

		pcl::visualization::CloudViewer viewer("Cloud Viewer");

		// blocks until the cloud is actually rendered
		viewer.showCloud(cloud);

		// use the following functions to get access to the underlying more advanced/powerful
		// PCLVisualizer

		// This will only get called once
		viewer.runOnVisualizationThreadOnce (viewerOneOff);

		// This will get called once per visualization iteration
		viewer.runOnVisualizationThread (viewerPsycho);
		while (!viewer.wasStopped ()) {
			// you can also do cool processing here
			// FIXME: Note that this is running in a separate thread from viewerPsycho
			// and you should guard against race conditions yourself...
		}


		return 0;
	}


create a new file called `cloud_viewer.cpp` , copy and paste it

![alt text](https://imgur.com/a/Z7Syp)


Using cmake to build PCL project 
