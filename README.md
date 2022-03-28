from cv2 import FONT_HERSHEY_COMPLEX_SMALL, cv2, resize

imagen = 'C:\Users\Carlos\Documents\Octavo\Robotica movil\circulo.png'
imagen1 = cv2.imread(imagen)
imagen2 = resize(imagen1, dsize=(600,600), interpolation= cv2.INTER_CUBIC)
gray = cv2.cvtColor(imagen2, cv2.COLOR_BGR2GRAY)
canny = cv2.Canny(gray, 10, 150)
canny = cv2.dilate(canny, None, iterations=1)
canny = cv2.erode(canny, None, iterations=1)
cnts,_ = cv2.findContours(canny, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)


for c in cnts:
	epsilon = 0.01*cv2.arcLength(c,True)
	approx = cv2.approxPolyDP(c,epsilon,True)
	#print(len(approx))
	x,y,w,h = cv2.boundingRect(approx)

	if len(approx)==4:
		cv2.putText(imagen2,'Cuadrado', (x-10,y),1,FONT_HERSHEY_COMPLEX_SMALL,(0,255,255),1)

	if len(approx)==10:
		cv2.putText(imagen2,'Estrella', (x-10,y),1,FONT_HERSHEY_COMPLEX_SMALL,(0,255,255),1)

	if len(approx)>10:
		cv2.putText(imagen2,'Circulo', (x-10,y),1,FONT_HERSHEY_COMPLEX_SMALL,(0,255,255),1)

	cv2.drawContours(imagen2, [approx], 0, (0,255,255),2)
	cv2.imshow('imagen2',imagen2)
	cv2.waitKey(0)
