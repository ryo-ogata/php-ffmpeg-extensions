# php-ffmpeg-extensions
An extensions library for the PHP FFMpeg (https://github.com/PHP-FFMpeg/PHP-FFMpeg)

Installation (via Composer):
============================

For composer installation, add:

```json
"require": {
    "sharapov/php-ffmpeg-extensions": "dev-master"
},
```

to your composer.json file and update your dependencies. Or you can run:

```sh
$ composer require sharapov/php-ffmpeg-extensions
```

Usage:
======

Now you can autoload or use the class via its namespace. Below are examples of how to use the library.

Draw texts and boxes
--------------------------

```php
require_once dirname(__FILE__) . '/../vendor/autoload.php';

// Init FFMpeg library
$ffmpeg = \FFMpeg\FFMpeg::create(array(
    'ffmpeg.binaries'  => '/usr/local/bin/ffmpeg', // Path to FFMpeg
    'ffprobe.binaries' => '/usr/local/bin/ffprobe', // Path to FFProbe
    'timeout'          => 3600, // The timeout for the underlying process
    'ffmpeg.threads'   => 12,   // The number of threads that FFMpeg should use
));

// Open source video
$video = $ffmpeg->open(new \Sharapov\FFMpegExtensions\Input\File(dirname(__FILE__) . '/source/demo_video_720p_HD.mp4'));

// Create complex filter collection
$options = new \Sharapov\FFMpegExtensions\Filters\Video\FilterComplexOptions\OptionsCollection();

// Create drawtext option 1
$text1 = new \Sharapov\FFMpegExtensions\Filters\Video\FilterComplexOptions\OptionDrawText();
$text1
    // Set z-index property. Greater value is always in front
    ->setZIndex(160)
    // Set font path
    ->setFontFile(new \Sharapov\FFMpegExtensions\Input\File(dirname(__FILE__) . '/source/calibri.ttf'))
    // Set font color. Accepts transparency value as the second argument. Float value between 0 and 1.
    ->setFontColor('#ffffff')
    // Set font size in pixels
    ->setFontSize(33)
    // Set text string
    ->setText('php-ffmpeg-extensions library')
    // Coordinates where the text should be rendered. Accepts positive integer or
    // constants "(w-tw)/2", "(h-th)/2" to handle auto-horizontal, auto-vertical values
    ->setCoordinates(new \Sharapov\FFMpegExtensions\Coordinate\Point(\Sharapov\FFMpegExtensions\Coordinate\Point::AUTO_HORIZONTAL, 50))
    // Set timings (start, stop) in seconds. Accepts float values as well
    ->setTimeLine(new \Sharapov\FFMpegExtensions\Coordinate\TimeLine(1, 20));

// Pass option to the options collection
$options
    ->add($text1);

// Create drawtext option 2
$text2 = new \Sharapov\FFMpegExtensions\Filters\Video\FilterComplexOptions\OptionDrawText();
$text2
    ->setZIndex(160)
    ->setFontFile(new \Sharapov\FFMpegExtensions\Input\File(dirname(__FILE__) . '/source/arial.ttf'))
    ->setFontColor('#ffffff')
    ->setFontSize(28)
    ->setText('Sharapov A. (www.sharapov.biz)')
    ->setCoordinates(new \Sharapov\FFMpegExtensions\Coordinate\Point(15, 600))
    ->setTimeLine(new \Sharapov\FFMpegExtensions\Coordinate\TimeLine(1, 20));

$options
    ->add($text2);

// Create drawbox option
$box = new \Sharapov\FFMpegExtensions\Filters\Video\FilterComplexOptions\OptionDrawBox();
$box
    ->setZIndex(130)
    ->setColor('black')
    ->setDimensions(new \Sharapov\FFMpegExtensions\Coordinate\Dimension(\Sharapov\FFMpegExtensions\Coordinate\Dimension::WIDTH_MAX, 60))
    ->setCoordinates(new \Sharapov\FFMpegExtensions\Coordinate\Point(0, 580))// Set coordinates
    ->setTimeLine(new \Sharapov\FFMpegExtensions\Coordinate\TimeLine(1, 20)); // Set timings (start, stop) in seconds

$options
    ->add($box);

// Create drawtext option 3
$text2 = new \Sharapov\FFMpegExtensions\Filters\Video\FilterComplexOptions\OptionDrawText();
$text2
    ->setZIndex(160)
    ->setFontFile(new \Sharapov\FFMpegExtensions\Input\File(dirname(__FILE__) . '/source/arial.ttf'))
    ->setFontColor('#ffffff')
    ->setFontSize(28)
    ->setText('v2.0')
    ->setCoordinates(new \Sharapov\FFMpegExtensions\Coordinate\Point(1200, 600))
    ->setTimeLine(new \Sharapov\FFMpegExtensions\Coordinate\TimeLine(1, 20));

$options
    ->add($text2);

// Apply filter options to video
$video
    ->filters()
    ->complex($options);

// Run render
$format = new \FFMpeg\Format\Video\X264('libmp3lame');
$format->on('progress', function ($video, $format, $percentage) {
  print 'Done: '.$percentage . "%\n";
});

$video
    ->save($format, dirname(__FILE__) . '/output/output.mp4');
```

You will find other examples in "/examples" folder. 

Changelog
=========


Links
=====
[PHP FFMpeg Homepage](https://github.com/PHP-FFMpeg/PHP-FFMpeg)

[Composer](https://getcomposer.org/)

[GitHub](https://github.com/sharapov-outsource/php-ffmpeg-extensions)

[Packagist](https://packagist.org/packages/sharapov/php-ffmpeg-extensions)
