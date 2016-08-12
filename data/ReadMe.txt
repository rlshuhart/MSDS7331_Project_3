Thanks for participating in KDD Cup 2008 - we look forward to an interesting competition!

Legal terms & conditions:
By downloading this data file you agree to abide by the terms & conditions for KDD Cup 2008 as listed athttp://www.kddcup2008.com/Terms_of_use.html - If you do not agree with the terms of the contest, please delete the data. 

Administrative details:
The participants may point out errors or request clarifications: please check the FAQ page at the competition web page first. Despite our best efforts to verify things before hand, occasionally bugs discovered by participants may cause a slight change in the data / supporting code provided from this site. We will try to inform all the registered participants about any major changes by email. Please check back every few weeks at the FAQ site as well. 

Description of the data:
Features.txt contains information for 102294 suspicious regions (candidates), each described by 117 features. Each region is described by 1 row, and each feature is one column. See the KDD Cup web site for more background description and information about how these suspicious candidates were generated. NOTE: We can not publicly disclose or describe the features provided for use in the KDD Cup. 

Info.txt contains additional information for each suspicious region. Each row provides information about the corresponding row number in Features.txt. The columns of Info.txt are described below. Note that the first three columns are provided for the training data (to help you develop better algorithms), but they will not be provided for the testing data that will be used to evaluate your algorithm's accuracy. Your algorithm can use the input features and the rest of the information in the Info.txt in order to diagnose the suspicious locations. For example you may develop methods which try to correlate suspicious regions (in a soft/approximate manner) across MLO/CC images of the same breast, in order to improve the diagnostic accuracy (for example, one popular approach considers the distance of the lesion to the nipple to be roughly preserved across MLO & CC images). More hints are provided at www.kddcup2008.com

Columns of Info.txt:
Col #1 Ground truth label (+1/-1) indicating whether or not that region is a malignant mass
Col #2 Image-Finding-ID contains a unique non-negative identifier for each suspicious lesion in an image which is actually cancerous. Multiple candidates may point to the same underlying cancerous region in the breast, and hence may share the same image finding ID. Note that each breast is viewed through two images (MLO and CC). The image finding ID would be different across the two images of the same breast even for findings that point to the same underlying lesion. Image finding ID is zero for all negative (non-malignant) lesions. 
Col #3 Study-Finding-ID is similar to image finding ID, but it uniquely identifies a lesion across both images of the breast. Thus if the same lesion were visible in MLO & CC images, the candidates corresponding to this lesion in the MLO image would share an image finding ID which would differ from the image finding ID for the candidates describing the same lesion as seen from te CC image. However, regardless of whether a candidate was identified from MLO or from CC it would share the same non-zero study-finding-ID if it pointed to the same underlying lesion on the breast. 
Col #4 Image-ID is a unique identifier for the image from which the candidate was generated. 
Col #5 Study-ID is a unique identifier for the patient from whom the candidate was generated. Note that each patient would have four images (right/left breast)x(MLO/CC), and candidates would typically be obtained from all 4 images. 
Col #6 LeftBreast (1/0) is 1 if the candidate was generated from the left breast.
Col #7 MLO is 1 if the candidate is obtained from an MLO image and 0 otherwise. 
Col #8 X-location gives the X-pixel location of the candidate on the image. 
Col #9 Y-location gives the Y-pixel location of the candidate on the image. 
Col #10 X-nipple-location gives the X-pixel location of the nipple on the image (as provided by proprietary image processing algorithms).
Col #11 Y-nipple-location gives the Y-pixel location of the nipple on the image.

Code:
Code for computing official KDD Cup Free Response ROC curves is provided in get_ROC_KDD.m (written in Matlab). This FROC differes slightly from usual ROC curves in terms of the normalization of axes: the X-axis is the average number of false alarms per image, and Y-axis is the sensitivity with which cancerous *patients* are detected by the algorithm.

Example use of the code:
For example, if you had previously trained a linear classifier (denoted as classifier.w), this Matlab code snippet may be used to evaluate the accuracy of the classifier as follows:

% Evaluate the classifier:
score=test.X*classifier.w;

% Compute the FROC and the are under this curve between [0.2-0.3] FA/image:
[Pd_patient_wise,FA_per_image,AUC]=get_ROC_KDD(score, test.Y, test.patientID);
figure; plot(FA_per_image*4,Pd_patient_wise)
title 'FROC';  axis([0 2 0 1]); grid on
ylabel('Per-patient sensitivity'); 
xlabel('False Alarms per case');

NOTE: The training/testing data have been generated from a patient population that is somewhat different from the normal screening population. We have enriched the cases mix with some difficult cases in order to make the KDD Cup challenge more interesting. While we provided features produced by a library of well known (public domain) image processing algorithms, we have *not* provided access to a large pool of (more powerful) proprietary features developed by Siemens Medical Solutions. The accuracy of KDD Cup participants should not be construed to be representative in any way of commercial Siemens products. 