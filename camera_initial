# HWSW-project
#include <cstdlib>
#include <cstring>
#include <iostream>
#include <opencv2/opencv.hpp>
#include <vector>

#include <sys/stat.h>
#include <unistd.h>

using namespace std;

#define WINDOW_NAME "TEST"
#define LAST_FRAME_FILENAME "test-last-frame.png"

// check the existence of a file
bool exists(const char * filename) {
  struct stat buffer;   
  return (stat (filename, &buffer) == 0); 
}

// check if x window is available.
bool check_x(void) {
  const char *x_env = getenv("DISPLAY");
  bool has_x = true;
  if (!x_env || !strlen(x_env)) {
    std::cerr << "No X DISPLAY destination specified, no showing the image" << std::endl;
    has_x = false;
  }
  return has_x;
}

int main(int argc, char *argv[]) {
  string gst_src("xlnxvideosrc src-type=\"mipi\" ! video/x-raw, framerate=30/1, width=1920, height=1080, format=YUY2 ! videoconvert ! appsink"); // gst src command
  const int test_time = 32;       // default test time

  // check if x window is set properly.
  bool has_x = check_x();

  // get image frame from a video stream
  cv::VideoCapture camera(gst_src);
  cv::Mat img;
  vector<cv::Mat> display_queue;

  for (size_t i = 0; i < test_time; i++) {
    camera >> img;
    display_queue.push_back(img);
  }
  camera.release();

  // only display when x window is available.
  if (has_x) {
    cv::namedWindow(WINDOW_NAME, CV_WINDOW_AUTOSIZE);
    for (size_t i = 0; i < display_queue.size(); i++) {
      cv::imshow(WINDOW_NAME, display_queue[i]);
      usleep(10000);
    }

    cv::waitKey(0);
  }

  // save last frame for checking
  cv::imwrite(LAST_FRAME_FILENAME, display_queue[display_queue.size() - 1]);
  cout << "cvtest-dev: Done." << endl;
  return 0;
}
