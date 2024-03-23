# QR_CODE_SCANNER
This Python programming is based on Computer Vision and Pyzbar library which will detect and display data of QR-Codes.


# importing necessary libraries
import cv2 as cv
import numpy as np
from pyzbar.pyzbar import decode

# accessing webcam
cap = cv.VideoCapture(0)    

while cap.isOpened():
    # reading the frame
    ret, frame = cap.read()
    # Display the frame
    cv.imshow('My_camera', frame)
    # Decode QR codes
    decoded = decode(frame)
    
    for qr in decoded:
        # converting data into string characters (unifrom transform code 8-bit)
        data = qr.data.decode('utf-8')
        
        # printing the data that is decoded from qr-code
        print(data)
        
        # Constructs a NumPy array pts containing the polygon coordinates of the QR code
        pts = np.array([qr.polygon], np.int32)
        
        # Reshapes the array pts to the required format for drawing polylines.
        pts = pts.reshape((-1, 1, 2)) 
        
        # Draws polylines around the QR code on the frame frame using the coordinates in pts.
        lines = cv.polylines(frame, [pts], True, (0,0,255), 3)
        # Displays the frame with polylines drawn around the QR code.
        cv.imshow('My_camera', lines)
        
        # displaying text around qr-code
        
        # Retrieves the rectangle coordinates of the QR code bounding box.
        pts2 = qr.rect
        # Draws text containing the decoded data at the top-left corner of the bounding box.
        text = cv.putText(frame, data, (pts2[0], pts2[1]), cv.FONT_HERSHEY_PLAIN, 3, (255,255,0), 2)
        cv.imshow('My_camera', text)
        
        
    # the camera window closes when 'q' is pressed.    
    if cv.waitKey(1) == ord('q'):
        break
        
# closes the camera window
cap.release()
# destroy all windows that were opened
cv.destroyAllWindows()
