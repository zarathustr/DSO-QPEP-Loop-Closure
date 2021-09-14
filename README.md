# QPEP-DSO: Quadratic Pose Estimation Problems (QPEP) Enhanced Direct Sparse Odometry with Loop Closure

## Code Arch
The codes are motivated by DSO (https://github.com/JakobEngel/dso) by Engel et al. and LDSO (https://github.com/tum-vision/LDSO) by Xiang Gao et al.

The compiler has to support C++14 standard.

## Build
First, come to the ```thirdparty``` directory and then compile ```g2o``` and ```DBoW3``` (DO NOT INSTALL!!!). Then, build the project. The final binaries locate at ```bin``` directory. 

Trouble shooting: 

1. ```Pangolin_FOUND is not TRUE```: This means that the Pangolin cmake file did not change the enviroment variable. Use ```cmake -DPangolin_FOUND=TRUE``` to gap this error.

2. ```/usr/bin/ld: cannot find -lEigen3::Eigen```: This means that the Pangolin cmake file has some problems, try this command and then rebuild: ```sudo ln -s /usr/local/lib/libpango_core.so /usr/local/lib/libEigen3::Eigen.so```

## Related Publications

![alt tag](odom.png)

 * **LDSO: Direct Sparse Odometry with Loop Closure**, *X. Gao, R. Wang, N. Demmel, D. Cremers*,
   In International Conference on Intelligent Robots and Systems (IROS), 2018.
 * **Direct Sparse Odometry**, *J. Engel, V. Koltun, D. Cremers*,
   In IEEE Transactions on Pattern Analysis and Machine Intelligence (PAMI), 2018
 * **A Photometrically Calibrated Benchmark For Monocular Visual Odometry**, *J. Engel, V. Usenko, D. Cremers*,
   In arXiv:1607.02555, 2016
 * **Quadratic Pose Estimation Problems: Unified Solutions, Solvability/Observability Analysis and Uncertainty Description in A Globally Optimal Framework**, *J. Wu, Y. Zheng, Z. Gao, Y. Jiang, X. Hu, Y. Zhu, J. Jiao, M. Liu*
   2020, https://github.com/zarathustr/QPEP

## Run
 - TUM-Mono Dataset: [https://vision.in.tum.de/mono-dataset](https://vision.in.tum.de/mono-dataset)

To run QPEP-DSOP on TUM-Mono dataset sequence 34, execute:

```
./bin/run_dso_tum_mono \
    preset=0 \
    files=XXXXX/TUMmono/sequences/sequence_34/images.zip \
    vignette=XXXXX/TUMmono/sequences/sequence_34/vignette.png \
    calib=XXXXX/TUMmono/sequences/sequence_34/camera.txt \
    gamma=XXXXX/TUMmono/sequences/sequence_34/pcalib.txt \
    method=QPEP-RANSAC
```

