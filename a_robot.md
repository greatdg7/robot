
package MyRobots;
import robocode.*;

import robocode.Robot;
import robocode.AdvancedRobot;
import robocode.ScannedRobotEvent;
import robocode.WinEvent;
import static robocode.util.Utils.normalRelativeAngleDegrees;
import java.awt.*;
import java.util.Scanner;

public class Eend extends AdvancedRobot {

	public void run() {
		// Set colors
		setBodyColor(Color.black);
		setGunColor(Color.red);
		setRadarColor(Color.black);
		setScanColor(Color.gray) ;
		setBulletColor(Color.red);
		
		setAdjustRadarForRobotTurn(true);
		setAdjustRadarForGunTurn(true);
		setAdjustGunForRobotTurn(true);
		
		while(true){
			turnRadarRight(20);
		}
	}


	public void onScannedRobot(ScannedRobotEvent e) {
		
		double eheading = e.getHeading();
		double ebearing = e.getBearing();
		double edistance = e.getDistance();
		double evelocity = e.getVelocity();
		
		
		//bereken verschil huidige vijand coördinaten
		double VrealXenemy = Math.cos(Math.toRadians(e.getHeading()-ebearing)*edistance);
		double VrealYenemy = Math.sin(Math.toRadians(e.getHeading()-ebearing)*edistance);
		//bereken huidige vijand coördinaten
		double realXenemy = VrealXenemy + getX();
		double realYenemy = VrealYenemy + getY();
		
		//bereken richtingscoëfficent uit richting vijand
		double RCpredictedenemy = Math.tan(Math.toRadians(eheading));
		//bereken snelheid in X richting
		double Xvelocity = Math.sin(Math.toRadians(eheading))*evelocity;
		//bereken verschil voorspelde enemy locatie t.o.v. huidige enemy locatie
		double VpredictedXenemy = Xvelocity;
		double VpredictedYenemy = Xvelocity*RCpredictedenemy;
		
		//bereken voorspelde enemy locatie
		double predictedXenemy = realXenemy + VpredictedXenemy;
		double predictedYenemy = realYenemy + VpredictedYenemy;
		
		//bereken verschil onze locatie en locatie enemy
		double VaimX = predictedXenemy - getX();
		double VaimY = predictedYenemy - getY();
		
		double gunangleTarget = Math.toDegrees(Math.atan2(VaimX, VaimY)/(0.5*Math.PI));
	
		int gunangle = (int)(gunangleTarget - getGunHeading());
		
		if(gunangleTarget < 1)
			setBodyColor(Color.pink);
			
		turnGunRight(gunangle);
		fire(0.0001);
	}


	public void onWin(WinEvent e) {
		// Victory dance
		while(true){
			turnRight(45);
			turnLeft(45);
		}
	}
}				
