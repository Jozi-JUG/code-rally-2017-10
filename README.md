# Getting Started With Code Rally

## Introduction
[CodeRally](https://www.ibm.com/developerworks/mydeveloperworks/blogs/code-rally/entry/landing?lang=en) is a competitive coding challenge where you develop a rally car to compete against other cars.
Your ability to develop a good algorithm will make the difference.

## Requirements

* Java 8 
* Eclipse Oxygen JEE 

## Installation

This [document](coderally.pdf) has a more detailed description that the steps below.

* [Java 8 JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Eclipse Oxygen JEE](http://www.eclipse.org/downloads/packages/release/Oxygen/R). Expand the archive to a location where you have permission to modify files and execute eclipse
  * [Windows 64 bit](http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/oxygen/R/eclipse-jee-oxygen-R-win32-x86_64.zip)
  * [Mac](http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/oxygen/R/eclipse-jee-oxygen-R-macosx-cocoa-x86_64.dmg)
  * [Linux 64 bit](http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/oxygen/R/eclipse-jee-oxygen-R-linux-gtk-x86_64.tar.gz) 
* IBM Bluemix Tools Eclipse plugin. Visit [http://marketplace.eclipse.org/content/ibm-eclipse-tools-bluemix](http://marketplace.eclipse.org/content/ibm-eclipse-tools-bluemix) and drag and drop the Install icon onto your Eclipse Workspace.
* Coderally Eclipse plugin. 
  * In Eclipse. 
  * Help -> Install New Software...
  * Add... -> Name "Code Rally" and Url [http://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/wasdev/coderally/downloads/eclipseplugin](http://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/wasdev/coderally/downloads/eclipseplugin). 
  * Uncheck 'Group Items by Category' and check the plugin and click install.
* WebSphere Liberty Profile
  * In Eclipse 
  * Window -> Open Perspective -> Java EE
  * Servers View -> New Server
  * IBM -> WebSphere Liberty Profile
  * Choose location where you have write access
  * Choose WebSphere Liberty Profile Web Profile Option.
  * Once installed Double-click on Server Configuration -> Feature Manager and add `websocket-1.1` and save,
  * Start the Server.
* CodeRallyWeb
  * Download WAR from [https://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/wasdev/coderally/downloads/CodeRallyWeb.war](https://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/wasdev/coderally/downloads/CodeRallyWeb.war)
  * In Eclipse
  * File -> Import -> WAR file
  * Select you downloaded file
  * Don't check and sub modules.
  * When imported, right-click on CodeRallyWeb -> Run As -> Run on Server and select your WebSphere Liberty Profile server.
  * Visit homepage on [http://localhost:9080/CodeRallyWeb](http://localhost:9080/CodeRallyWeb)
  
    
## Your First Car

Some people have experienced issue with the Intermediate car.
This is the kind that the plugin will generate when you create your first agent car.

*Always start the WebSphere Liberty Server before creating a car or making any changes to your car. It seems once the plugin has failed to connect it doesn't try again.*  

This is a very basic car. You can change the behaviour by remove call to `super` and performing alternative logic.

```java
import com.ibm.coderally.agent.DefaultCarAIAgent;
import com.ibm.coderally.api.agent.AIUtils;
import com.ibm.coderally.entity.cars.agent.Car;
import com.ibm.coderally.entity.obstacle.agent.Obstacle;
import com.ibm.coderally.geo.agent.CheckPoint;
import com.ibm.coderally.track.agent.Track;

public class VeryBasicCar extends DefaultCarAIAgent {

	@Override
	public void onCarCollision(Car other) {
		super.onCarCollision(other);		
	}

	@Override
	public void onCheckpointUpdated(CheckPoint oldCheckpoint) {
		getCar().setBrakePercent(0);
		getCar().setAccelerationPercent(100);
		getCar().setTarget(AIUtils.getClosestLane(getCar().getCheckpoint(), getCar().getPosition()));
	}

	@Override
	public void onObstacleInProximity(Obstacle obstacle) {
		super.onObstacleInProximity(obstacle);		
	}

	@Override
	public void onOnTrack() {
        super.onOnTrack();		
	}
	
	@Override
	public void onOffTrack() {
		super.onOffTrack();		
	}

	@Override
	public void onOpponentInProximity(Car car) {
		super.onOpponentInProximity(car);		
	}

	@Override
	public void onRaceStart() {

		// Replace with custom logic or remove method for default implementation.

		getCar().setBrakePercent(0);
		getCar().setAccelerationPercent(100);
		getCar().setTarget(AIUtils.getClosestLane(getCar().getCheckpoint(), getCar().getPosition()));
		
	}

	@Override
	public void onTimeStep() {		
		
		AIUtils.recalculateHeading(getCar());
	}

	@Override
	public void init(Car car, Track track) {
		super.init(car, track);		
	}

	@Override
	public void onObstacleCollision(Obstacle obstacle) {
		super.onObstacleCollision(obstacle);
	}

	@Override
	public void onStalled() {
        super.onStalled();		
	}
}
```

## GameServer

Visit [http://rally.cloudafrica.net:9080/CodeRallyWeb/](http://rally.cloudafrica.net:9080/CodeRallyWeb/)