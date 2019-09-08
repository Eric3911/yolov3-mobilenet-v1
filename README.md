								# YOLOv3-Mobilenet

**1. In this version, we found that the error of Val = 0 occurred in the code during our training process, which made it impossible to train.

issue:  The error code fragment in the  train_Mobilenet.py   script is as follows：

	with open(train_path) as t_f:
        t_lines = t_f.readlines()
    np.random.seed(10101)
    np.random.shuffle(t_lines)
    np.random.seed(None)
    v_lines = t_lines[700:]
    t_lines = t_lines[:700]
    num_train = len(t_lines)
  
exchange: We modified the code to be trained as follows：

    with open(train_path) as t_f:
        t_lines = t_f.readlines()
        random_stat = 123
        np.random.seed(random_stat)
        t_lines, v_lines = train_test_split(t_lines, test_size=0.2, random_state=random_stat)
        num_train = len(t_lines)

  
 **2.Practical steps of using transfer learning training model:
 
A) Put all the label XML of training data in Annotations;
B) Put all the labeled training pictures in JPEG Images;
C) Use the creat_list.py script under VOC2007 to generate four new documents in Main under ImageSets;
D) Write the class parameters in the sixth line of the voc_annotation.py script under the root directory as their own class parameters in ["aircraft"];
E) Run the voc_annotation.py script code to generate three files in the root directory: 2007_test; 2007_train; 2007_val;
F) Modify the class label in voc_classes under model_data directory, where the order must always be and must be the same as that in classes, including the space placeholders between some label words.
G) Run the kmeans script to generate new yolo_anchors and copy them to the model_data directory to overwrite the previous yolo_anchors;
H) Modify yolov3.cfg, which is as common as other modifications.
I) Use the train_Mobilnet.py code to modify the path of its relevant parameters. Generally, at 21-24, 37, 68, 74, 75, 86, 92, 93 lines of code, the script model can be trained to execute.
J) After the training, yolo_Mobilnet is used to test the model and get the result.
