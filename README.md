# YOLOv3-Mobilenet

一、在本版本中我们对发现该段代码在我们训练的过程中出现val = 0的这种报错导致没法训练

报错代码片段为train_Mobilenet.py 脚本中报错代码片段如下：
	with open(train_path) as t_f:
        t_lines = t_f.readlines()
    np.random.seed(10101)
    np.random.shuffle(t_lines)
    np.random.seed(None)
    v_lines = t_lines[700:]
    t_lines = t_lines[:700]
    num_train = len(t_lines)
  
  
我们修改为如下代码代码可以训练：
    with open(train_path) as t_f:
        t_lines = t_f.readlines()
        random_stat = 123
        np.random.seed(random_stat)
        t_lines, v_lines = train_test_split(t_lines, test_size=0.2, random_state=random_stat)
        num_train = len(t_lines)
   
 
 
 二、我们将我们修改的脚本改为train_Mobilnet_V2.py
 
 
 
 三、使用迁移学习训练模型实践步骤：
    a）将训练训练的数据的标签xml全部放在Annotations；
    b）将标签好训练训练的图片全部放在JPEGImages；
    c）使用VOC2007下面的creat_list.py脚本生成ImageSets下Main里面四个全新的文档；
    d）到根目录下面的voc_annotation.py的脚本下第六行代码 classes = ["aircraft", "oiltank" ] 里面的类别参数写成自己的类别参数；
    e）运行voc_annotation.py脚本代码生成根目录下的三个文件list：2007_test;2007_train;2007_val；
    f）修改model_data目录下面的voc_classses 里面的类别标签，这里和classes里面顺序必须一直并且必须保证和标记的xml一直包括有些标签单词之间的空格占位符都要一样；
    g）运行kmeans脚本生成新的yolo_anchors并将其复制到model_data目录下面覆盖之前的yolo_anchors；
    h）修改yolov3.cfg这个和其他一样是通用的修改几个地方；
    i）使用train_Mobilnet_V2.py代码修改为自己的相关参数路径，一般修改的地方21-24，37，68，74，75，86，92，93行代码处，执行该脚本模型就能训练起来。
	j）训练结束后使用yolo_Mobilnet_V2进行模型测试得到结果
