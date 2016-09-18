Ref: http://blog.miguelgrinberg.com/post/how-to-build-and-run-mjpg-streamer-on-the-raspberry-pi

1. Install build dependencies
    sudo apt-get install libjpeg8-dev imagemagick libv4l-dev

2. Add missing videodev.h
    sudo ln -s /usr/include/linux/videodev2.h /usr/include/linux/videodev.h

3. Download MJPG-Streamer (you can skip this step if you already have download it. I attached it in this reposistory)
    wget http://sourceforge.net/code-snapshots/svn/m/mj/mjpg-streamer/code/mjpg-streamer-code-182.zip

4. Unzip the MJPG-Streamer source code
    unzip mjpg-streamer-code-182.zip

5. Build MJPG-Streamer
    cd mjpg-streamer-code-182/mjpg-streamer
    make mjpg_streamer input_file.so output_http.so

6. Install MJPG-Streamer
    sudo cp mjpg_streamer /usr/local/bin
    sudo cp output_http.so input_file.so /usr/local/lib/
    sudo cp -R www /usr/local/www
 
7. Start the camera
    mkdir /tmp/stream
    raspistill --nopreview -w 640 -h 480 -q 5 -o /tmp/stream/pic.jpg -tl 100 -t 9999999 -th 0:0:0 &

open another console to connect to raspberry board and run this command to start MJPG-Streamer:
    LD_LIBRARY_PATH=/usr/local/lib mjpg_streamer -i "input_file.so -f /tmp/stream -n pic.jpg" -o "output_http.so -w /usr/local/www"


To watch video stream on local host by using brower (recommend to use firefox):
    http://<ip board raspberry>:8080
choice stream tab 


8. Cleanup
    cd ../../
    rm -rf mjpg-streamer-182
