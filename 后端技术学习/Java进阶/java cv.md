java cv
---

<!-- TOC -->

- [1. Maven依赖](#1-maven依赖)
- [2. Java CV的核心代码](#2-java-cv的核心代码)
  - [2.1. 获取视频时长](#21-获取视频时长)
  - [2.2. 接取指定视频帧](#22-接取指定视频帧)
  - [2.3. FFmpegFrameGrabber详解](#23-ffmpegframegrabber详解)
  - [2.4. Java2DFrameConverter](#24-java2dframeconverter)
  - [2.5. BufferedImage](#25-bufferedimage)
- [3. java cv 参考](#3-java-cv-参考)

<!-- /TOC -->

# 1. Maven依赖
```xml
<dependency>
    <groupId>org.bytedeco</groupId>
    <artifactId>javacv-platform</artifactId>
    <version>1.4.4</version>
</dependency>
```

# 2. Java CV的核心代码

## 2.1. 获取视频时长
```java
/**
 * 获取视频时长，单位为秒
 *
 * @param video 源视频文件
 * @return 时长（s）
 */
public static long getVideoDuration(File video) {
    long duration = 0L;
    FFmpegFrameGrabber ff = new FFmpegFrameGrabber(video);
    try {
        ff.start();
        duration = ff.getLengthInTime() / (1000 * 1000);
        ff.stop();
    } catch (FrameGrabber.Exception e) {
        e.printStackTrace();
    }
    return duration;
    }
```

## 2.2. 接取指定视频帧
```java
/**
 * 截取视频获得指定帧的图片
 *
 * @param video   源视频文件
 * @param picPath 截图存放路径
 */
public static void getVideoPic(File video, String picPath) {
    FFmpegFrameGrabber ff = new FFmpegFrameGrabber(video);
    try {
        ff.start();
        // 截取中间帧图片(具体依实际情况而定)
        int i = 0;
        int length = ff.getLengthInFrames();
        int middleFrame = length / 2;
        Frame frame = null;
        while (i < length) {
            frame = ff.grabFrame();
            if ((i > middleFrame) && (frame.image != null)) {
                break;
            }
            i++;
        }

        // 截取的帧图片
        Java2DFrameConverter converter = new Java2DFrameConverter();
        BufferedImage srcImage = converter.getBufferedImage(frame);
        int srcImageWidth = srcImage.getWidth();
        int srcImageHeight = srcImage.getHeight();

         // 对截图进行等比例缩放(缩略图)
        int width = 480;
        int height = (int) (((double) width / srcImageWidth) * srcImageHeight);
        BufferedImage thumbnailImage = new BufferedImage(width, height, BufferedImage.TYPE_3BYTE_BGR);
        thumbnailImage.getGraphics().drawImage(srcImage.getScaledInstance(width, height, Image.SCALE_SMOOTH), 0, 0, null);
    
        File picFile = new File(picPath);
        ImageIO.write(thumbnailImage, "jpg", picFile);

        ff.stop();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
public static void main(String[] args) {
    String videoPath = ResourceUtils.CLASSPATH_URL_PREFIX + "video.mp4";
    File video = null;
    try {
        video = ResourceUtils.getFile(videoPath);
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }
    String picPath = "video.jpg";
    getVideoPic(video, picPath);

    long duration = getVideoDuration(video);
    System.out.println("videoDuration = " + duration);
}
```

## 2.3. FFmpegFrameGrabber详解
方法名|功能|备注
--|--|--
hasVideo()|检查是否有视频文件|-
hasAudio()|检查是否有音频文件|-
getGamma()|-|-
getFormat()|-|-
getImageWidth()|获得图像宽度|-
getImageHeight()|获得图像高度|-
getAudioChannels()|获取音频声道|需要检查

## 2.4. Java2DFrameConverter

## 2.5. BufferedImage

# 3. java cv 参考
1. <a href = "https://blog.csdn.net/zs770635620/article/details/80387932">问题解决</a>
2. <a href = "https://blog.csdn.net/qq_28804275/article/details/88051306?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task">Java 获取视频时长及截取帧截图</a>