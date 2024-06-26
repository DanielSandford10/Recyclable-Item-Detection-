# Recyclable-Item-Detection

Make sure you have downloaded Jetson Inference and Docker Image from here: https://github.com/dusty-nv/jetson-inference

This is a algorithm that can identify types of trash and categroize them into recycycling and garbage

FOLLOW README IF YOU WOULD LIKE TO TRAIN FOR A HIGHER ACCURACY THAN THE CURRENT RELEASE, OTHERWISE, DOWNLOAD THE RELEASE FILE, EXPAND IT, AND RUN IT.

https://youtu.be/_om5nzDifnI

![](https://publicinterestnetwork.org/wp-content/uploads/2021/09/Photo-credit-Michael-Courier_0-scaled.jpeg)
# The Algorithm

It uses training data of peices of trash and uses image net to categorize them into whether they can be recycled or not. It can be used in a live and pre photo modes. It runs off of a a NVIDIA Jetson Nano.

I have also attached a train.py file to make your own.

# Running this project
## Training a Network
1. Open VSCode and sign in to your Jetson Nano
2. Make sure you have downloaded Jetson Inference and Docker Image from here: https://github.com/dusty-nv/jetson-inference
3. Download the file from the DropBox in the google doc and import it into your library on the device your running the info on.( Make sure its located in the jetson-inference/python/training/classification/data )
4. cd back to nvidia/jetson-inference/
5. Once that has run and you're still back in the jetson-inference folder, run ./docker/run.sh to run the docker container. (You may have to re-enter your nvidia password at this step)
6. From inside the Docker container, change directories so you are in jetson-inference/python/training/classification
7. Now you are ready to run your script.

Run the training script to re-train the network where the model-dir argument is where the model should be saved and where the data is. 
python3 train.py --model-dir=models/Recycling_Trash data/Recycling_Trash
 
You should immediately start to see output, but it will take a very long time to finish running. It could take hours depending on how many epochs you run for your model.
 
When running the model you can also specify the value of how many epochs and batch sizes you want to run. For example at the end of that code you can add:
--batch-size=NumberOfBatchFiles --workers=NumberOfWorkers --epochs=NumberOfEpochs

8. While it's running, you can stop it at any time using Ctl+C. You can also restart the training again later using the --resume and --epoch-start flags, so you don't need to wait for training to complete before testing out the model.

Run python3 train.py --help for more information about each option that's available for you to use, including other networks that you can try with the --arch flag.
## Exporting the Network
9. Make sure you are in the docker container and in jetson-inference/python/training/classification
10. Run the onnx export script.
python3 onnx_export.py --model-dir=models/Recycling_Trash
11. Look in the jetson-inference/python/training/classification/models/Recyling_Trash folder to see if there is a new model called resnet18.onnx there. That is your re-trained model!
## Processing Images
12. Exit the docker container by pressing Ctl + D.
13. On your nano, navigate to the jetson-inference/python/training/classification directory.
14. Use ls models/Recycling_Trash/ to make sure that the model is on the nano. You should see a file called resnet18.onnx.
15. Set the NET and DATASET variables
    NET=models/Recycling_Trash
    DATASET=data/Recycling_Trash
16. Run this command to see how it operates on an image from the test folder.
    imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/garbage/trash1.jpg TestImage1.jpg
17. Open the image in VS Code.




