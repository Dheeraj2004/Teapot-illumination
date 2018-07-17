# Teapot-illumination
Teapot illumination using visual studio
// Illumination.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include<gl/glut.h>

float radius = 1, yRotated= 0.1;
int flag = 1, sub_menu1, sub_menu2;
GLfloat ambspec1[] = {1,0,0,0},ambspec2[]={0,1,0,0},ambspec3[]={0,0,1,0},ambspec4[]={1,1,0,0}, light_pos[]={0,0,1};

void init() {
	glClearColor(0,0,0,1);
	glPolygonMode(GL_FRONT_AND_BACK,GL_FILL);
	glEnable(GL_LIGHTING);
	glEnable(GL_LIGHT0);
	glEnable(GL_NORMALIZE);
	glShadeModel(GL_SMOOTH);
}

void planet() {
	glPushMatrix();
	glTranslatef(0,0,0);
	glScalef(0.1,0.1,0.1);
	glRotatef(yRotated, 0,1,0);
	if(flag == 1) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec1);
	else if(flag == 2) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec2);
	else if(flag == 3) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec3);
	else glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec4);
	glMaterialf(GL_FRONT, GL_SHININESS, 80.0);
	glLightfv(GL_LIGHT0, GL_POSITION,light_pos);
	glutSolidSphere(radius,20,200);
	glPopMatrix();
}

void cube() {
	glPushMatrix();
	glTranslatef(0,-10,0);
	glRotatef(yRotated, 0,1,0);
	if(flag == 1) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec1);
	else if(flag == 2) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec2);
	else if(flag == 3) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec3);
	else glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec4);
	glMaterialf(GL_FRONT, GL_SHININESS, 80.0);
	glLightfv(GL_LIGHT0, GL_POSITION,light_pos);
	glutSolidCube(radius);
	glPopMatrix();
}

void dodec() {
	glPushMatrix();
	glTranslatef(0,-10,0);
	glRotatef(yRotated, 0,1,0);
	if(flag == 1) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec1);
	else if(flag == 2) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec2);
	else if(flag == 3) glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec3);
	else glMaterialfv(GL_FRONT, GL_AMBIENT_AND_DIFFUSE, ambspec4);
	glMaterialf(GL_FRONT, GL_SHININESS, 80.0);
	glLightfv(GL_LIGHT0, GL_POSITION,light_pos);
	glScalef(0.1,0.1,0.1);
	glutSolidDodecahedron();
	glPopMatrix();
}



void display() {
	glClear(GL_COLOR_BUFFER_BIT);
	glLoadIdentity();
	planet();
	glFlush();
}

void reshape(int w, int h) {
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	if(w<=h) gluPerspective(90,float(w)/float(h),2.0,20.0);
	else gluPerspective(90,float(h)/float(w),2.0,20.0);
	glMatrixMode(GL_MODELVIEW);
	glEnable(GL_DEPTH_TEST);
}

void rotate() {
	yRotated += 0.1;
	glutPostRedisplay();
}

void mouse(int button, int state, int x, int y) {
	if(button == GLUT_LEFT_BUTTON && state == GLUT_DOWN) {
		flag += (flag+1)%4;
	}
	glutPostRedisplay();
}

void menu(int n) {
	if(n==0) exit(0);
	else {
		switch(n) {
			case 1: 
				cube();
				break;
			case 2:
				planet();
				break;
			case 3:
				dodec();
				break;
			case 4:
				glPolygonMode(GL_FRONT_AND_BACK, GL_POINT);
				break;
			case 5:
				glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);
				break;
			case 6:
				glPolygonMode(GL_FRONT_AND_BACK, GL_FILL);
				break;
		}
	}
}

int main(int argc, char **argv) {
	glutInit(&argc, argv);
	glutInitDisplayMode(GLUT_RGB);
	glutInitWindowPosition(0,0);
	glutInitWindowPosition(500,500);
	glutCreateWindow("Planet");
	init();
	glutDisplayFunc(display);
	glutReshapeFunc(reshape);
	glutIdleFunc(rotate);
	glutMouseFunc(mouse);
	//SUB-MENU 1
	sub_menu1 = glutCreateMenu(menu);
	glutAddMenuEntry("Cube",1);
	glutAddMenuEntry("Sphere",2);
	glutAddMenuEntry("Dodecahedron",3);
	//SUB-MENU 2
	sub_menu2 = glutCreateMenu(menu);
	glutAddMenuEntry("Points",4);
	glutAddMenuEntry("Lines",5);
	glutAddMenuEntry("Fill", 6);
	//MAIN MENU
	glutCreateMenu(menu);
	glutAddSubMenu("Change Shape", sub_menu1);
	glutAddSubMenu("Change Layout", sub_menu2);
	glutAddMenuEntry("Quit", 0);
	glutAttachMenu(GLUT_RIGHT_BUTTON);
	glutMainLoop();
}
