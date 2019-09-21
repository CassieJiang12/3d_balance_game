//Yizhi Jiang

#include <gl/glut.h>
#include <cmath>
#include <iostream>
int width = 800, height = 800;
GLfloat pos[] = { 0, 3, 5, 1},
amb[] = { 0.3, 0.3, 0.3, 1.0 };
GLfloat pivote_diff[] = { 0.2, 0.3, 0.5, 1.0 },
		board_diff[] = { 1, 0.3, 0.5, 1.0 },
		sphere_left_diff[] = { 0.6, 0.4, 0.2, 1.0 },
		sphere_right_diff[] = { 0.2, 0.5, 0.5, 1.0 },
		face_diff[] = { 0.6, 0.2, 0.7, 1.0 };
GLfloat spe[] = { 0.25, 0.25, 0.25, 1.0 };
GLfloat theta = 0, dt = 0.5;
GLfloat face_translation[] = { 0, 0, 0 }, face_rotation = 0, face_scaling = 1.5;
GLfloat rotateAngle = 0;
GLUquadricObj *face, *eye_left, *eye_right, *mouth;
GLfloat right_sph_ver = 0, right_sph_hor = 0,
		left_sph_ver = 0, left_sph_hor = 0;
bool left = false, right = false, space = false, 
	 face_display = false,face_right = true, face_left = false, happy;
//Function to draw face. Then the seesaw balanced, it is a smiley face. Otherwise, it is a sad face.
void draw_face() {
	glPushMatrix();
	//Draw two eyes
	happy ? glTranslated(-0.35, 1, 0) : glTranslated(-0.35, 2.5, 0);
	glTranslated(face_translation[0], face_translation[1], face_translation[2]);
	glRotated(face_rotation, 0, 1, 0);
	glScaled(face_scaling, face_scaling, face_scaling);
	eye_left = gluNewQuadric();
	gluQuadricDrawStyle(eye_left, GLU_FILL);
	gluQuadricNormals(eye_left, GLU_SMOOTH);
	glScaled(0.2, 0.18, 0.18);
	gluDisk(eye_left, 0.35, 1.0, 48, 48);
	eye_right = gluNewQuadric();
	glTranslated(3, 0, 0);
	gluQuadricDrawStyle(eye_right, GLU_FILL);
	gluQuadricNormals(eye_right, GLU_SMOOTH);
	gluDisk(eye_right, 0.35, 1.0, 12, 8);
	glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, face_diff);
	//Draw the mouth
	happy ? glTranslated(-1.45, -0.8, 0) : glTranslated(-1.5, -5, 0);
	mouth = gluNewQuadric();
	gluQuadricDrawStyle(mouth, GLU_FILL);
	gluQuadricNormals(mouth, GLU_SMOOTH);
	glRotated(160, 1, 0, 0);
	glRotated(150, 0, 1, 0);
	glRotated(80, 0, 1, 1);
	glScaled(1.8, 1.8, 1.8);
	//Mouth of the smiley face
	if (happy)	gluPartialDisk(mouth, 1.5, 1.8, 12, 8, 0, 120);
	else { //Mouth of the sad face
		glRotated(180, 0, 0, 1);
		gluPartialDisk(mouth, 1.5, 1.8, 12, 8, 0, 115);
	}
	glPopMatrix();
}

void display(void) {
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	glPushMatrix();
	glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, pivote_diff);
	glMaterialf(GL_FRONT_AND_BACK, GL_SHININESS, 75);
	glLightfv(GL_LIGHT0, GL_AMBIENT, amb);
	glRotated(20, 1, 0, 0); //Draw the pivote cube 
	glTranslated(0, -2, 0);
	glutSolidCube(0.5);
	glPopMatrix();
	glPushMatrix();	//Draw the board
	glRotated(rotateAngle, 0, 0, 1);
	glPushMatrix();
	glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, board_diff);
	glRotated(20, 1, 0, 0);
	glTranslated(0, -1.5, 0);
	glScaled(12, 0.2, 0.5);
	glutSolidCube(0.6);
	glPopMatrix();
	glPushMatrix(); 	//Draw two spheres 
	glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, sphere_left_diff);
	if (left) { 	//If the left or right sphere is lifted, then rotate the light around z
		glPushMatrix();
		glRotated(theta, 0, 0, 1);
		glLightfv(GL_LIGHT0, GL_POSITION, pos);
		glPopMatrix();
	}
	glScaled(0.5, 0.5, 0.5);
	glTranslated(0.65, -2, 0); //Set original sphere positions
	//When the sphere is moved, update the position
	glTranslated(right_sph_hor / 10, right_sph_ver / 10, 0); 
	glutSolidSphere(0.6, 96, 96);
	glPopMatrix();
	glPushMatrix();
	glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, sphere_right_diff);
	if (right) { //Rotate the light around z
		glPushMatrix();
		glRotated(theta, 0, 0, 1);
		glLightfv(GL_LIGHT0, GL_POSITION, pos);
		glPopMatrix();
	}
	glScaled(0.5, 0.5, 0.5);
	glTranslated(-0.65, -2, 0); 
	glTranslated(-left_sph_hor / 10, left_sph_ver / 10, 0); 
	glutSolidSphere(0.6, 96, 96);
	glPopMatrix();
	glPopMatrix();
	//When it's balanced, draw the smily or sad face according to the result
	if (face_display) 
		draw_face();
	glutSwapBuffers();
}
//Default face animation when the seesaw is balanced
void idle_face(void) { 
	face_diff[0] = (face_diff[0] < 0.6) ? face_diff[0] += 0.0002 : 0;
	face_diff[1] = (face_diff[1] > 0) ? face_diff[1] -= 0.0002 : 0.6;
	face_diff[2] = (face_diff[2] > 0) ? face_diff[2] -= 0.0005 : 0.7;
	if (face_translation[0] > 2)	face_right = false;
	if (face_translation[0] < -2)	face_right = true;
	if (face_right) {
		face_translation[0] += 0.008;
		face_rotation+= 0.5;
		face_scaling = (face_translation[0] > 0) ? face_scaling - 0.005 : face_scaling + 0.005;
	}
	else {
		face_translation[0] -= 0.008;
		face_rotation-= 0.5;
		face_scaling = (face_translation[0] > 0) ? face_scaling + 0.005 : face_scaling - 0.005;
	}
//If it is not balanced, default sad face animation will be displayed
	face_translation[1] = happy ? (face_translation[0] * face_translation[0]) / 2
						  :-(face_translation[0] * face_translation[0]) / 2;
	glutPostRedisplay();
}
//If the left sphere is further, two spheres would drop tp left
void idle_to_left(void) {
	if (space && !(left_sph_hor == 0 && right_sph_hor == 0)) {
		if (left_sph_hor != 200)
			left_sph_hor += 0.25;
		if (right_sph_hor != -200)
			right_sph_hor -= 0.25;
		glutPostRedisplay();
	}
	if (left_sph_hor == 200 && right_sph_hor == -200 && rotateAngle != 0)
		rotateAngle -= 0.5;
	idle_face(); //Invoke the sad face animation
	glutPostRedisplay();
}
//If the right sphere is further, two spheres would drop tp right
void idle_to_right(void) {
	if (space && !(left_sph_hor == 0 && right_sph_hor == 0)) {
		if (left_sph_hor != -200)
			left_sph_hor -= 0.25;
		if (right_sph_hor != 200)
			right_sph_hor += 0.25;
		glutPostRedisplay();
	}
	if (left_sph_hor == -200 && right_sph_hor == 200 && rotateAngle != 0)
		rotateAngle += 0.5;
	idle_face(); //Invoke the sad face animation
	glutPostRedisplay();
}
//When the left sphere is lifted, rotate the light around z
void idle_left_sphere(void) {
	if (left_sph_ver < 30) left_sph_ver += 0.1;
	if (right_sph_ver > 0) right_sph_ver -= 0.1;
	theta = (theta < 360) ? theta + dt : dt;
	glutPostRedisplay();
}
//When the right sphere is lifted, rotate the light around z
void idle_right_sphere(void) {
	if (left_sph_ver > 0) left_sph_ver -= 0.1;
	if (right_sph_ver < 30) right_sph_ver += 0.1;
	theta = (theta > 0) ? theta - dt : 360;
	glutPostRedisplay();
}

void keyboard(unsigned char key, int x, int y) {
	switch (key) {
	case 'l': //When the 'l' key is pressed, lift the left sphere
		if (!space) { //Avoid lifting the sphere after balanced 
			left = true;
			right = false;
			glutIdleFunc(idle_left_sphere);
		}
		break;
	case 'r'://When the 'r' key is pressed, lift the right sphere
		if (!space) {
			left = false;
			right = true;
			glutIdleFunc(idle_right_sphere);
		}
		break;
	case ' ': //When the space bar is pressed, balance the seesaw
		right_sph_ver = 0; //Drop two spheres
		left_sph_ver = 0;
		left = false;
		right = false;
		space = true;
		if(!(left_sph_hor == 0 && right_sph_hor == 0))	
		//Avoid evaluating when the two spheres are at the original positions
			face_display = true; 
		//If the seesaw is balanced, display the smiley face
		if (abs(right_sph_hor - left_sph_hor) <= 3) {
			rotateAngle = 0;
			happy = true;
			glutIdleFunc(idle_face);
		}
		//If the seesaw is not balanced, display the sad face
		else if (right_sph_hor > left_sph_hor) {
			rotateAngle = -20;
			happy = false;
			glutIdleFunc(idle_to_right);
		}
		else if (right_sph_hor < left_sph_hor) {
			rotateAngle = 20;
			happy = false;
			glutIdleFunc(idle_to_left);
		}
		break;
	case '0': //When the '0' is pressed, restart the game
		face_display = false;
		space = false;
		rotateAngle = 0;
		right_sph_hor = 0;
		left_sph_hor = 0;
		break;
	}
	glutPostRedisplay();
}

void special_key(int key, int x, int y) {
	switch (key) {
	case GLUT_KEY_LEFT: //When the left key is pressed, move the sphere to left
		if (left && left_sph_hor < 50)	left_sph_hor++;
		if (right && right_sph_hor > 0)	right_sph_hor--;
		break;
	case GLUT_KEY_RIGHT://When the right key is pressed, move the sphere to right
		if (right && right_sph_hor < 50)
			right_sph_hor++;
		if (left && left_sph_hor > 0)
			left_sph_hor--;
		break;
	}
	glutPostRedisplay();
}

void main(int argc, char** argv) {
	std::cout << "********Seesaw Game********\n\nPress 'l' to lift up the left sphere.\n"
		<< "Press 'r' to lift up the right sphere.\n"
		<< "press LEFT ARROW '<-' to move th e current sphere left.\n"
		<< "press RIGHT ARROW '->' to move the current sphere right.\n"
		<< "press SPACE BAR to drop the sphere to balance.\n"
		<< "press '0' to restart the game.\n\n"
		<< "********ENJOY THE GAME********\n";
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
	glutInitWindowSize(800, 800);
	glutInitWindowPosition(200, 100);
	glutCreateWindow("Seesaw Yizhi Jiang");
	glClearColor(0.7, 0.7, 0.7, 0.0);
	glEnable(GL_DEPTH_TEST);
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluPerspective(80, 1.0, 3, 10);
	glLightModeli(GL_LIGHT_MODEL_TWO_SIDE, GL_TRUE);
	glLightf(GL_LIGHT0, GL_LINEAR_ATTENUATION, 0.08);
	glMatrixMode(GL_MODELVIEW);
	glLoadIdentity();
	glTranslated(0, 0, -5);
	glLightfv(GL_LIGHT0, GL_POSITION, pos);
	glEnable(GL_LIGHTING);
	glEnable(GL_LIGHT0);
	glutDisplayFunc(display);
	glutKeyboardFunc(keyboard);
	glutSpecialFunc(special_key);
	glutMainLoop();
}
