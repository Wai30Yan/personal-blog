---
title: "Why you might not wanna share photos online so easily"
date: 2023-02-05
draft: false
description: "All the shortcodes available in Blowfish."
slug: "exif-metadata"
tags: ["kali-linux", "cyber-security"]
showEdit: false
showAuthor: true
---
{{< lead >}}
Ever wonder whenever photos of criminals were shared, FBI arrested them the very next day? Photos contain metadata aka EXIF(Exchangeable Image File Format) which you do not see. That's because metadata are stored in machine readable code. Metadata are usually the keys to protect copyright and licensing of digital artworks. 
{{< /lead >}}

### How do you read metadata?

If you are a linux user. It is quite easy. All you need is a tool call `libimage-exiftool-perl`. If you don't have it on your machaine, you can install it by

```go
$ sudo apt install libimage-exiftool-perl
```

After installation, you can start using the tool with the command `exiftool`. I will use a photo of me during a hiking trip in Kachin State, Myanmar. 

First, you need to navigate to the folder where you keep the photo. Then, use the command with the name of the photo file.

```go
$ exiftool {name of the file}
```

{{< figure
    src="terminal.png"
    alt="terminal"
    default=true
    caption="Notice the current directory is \'Desktop/WaiYan\'"
    >}}

### The output of the `exiftool` command

After executing the command, you will see a bunch of information such as color data, date of taking the photo, shutter speed, exposure etc. In the image below, I've highlighted the Camera Model Name and Create Date. 

{{< figure
    src="meta1.jpg"
    alt="meta"
    default=true
    >}}

### The juicy part (GPS location) :satisfied:

At the very end, you will see the GPS location in longitude and latitude. If you convert them into decimal format, you can use the output in google map. The is one of the steps that law enforcement use to track criminals :smile:.

{{< figure
    src="meta2.png"
    alt="meta"
    default=true
    >}}

### What if I don't use Linux? Is there another way to read metadata?

If you don't use Linux, there are some websites that can do the same thing for free. There is one call [Jimple](https://jimpl.com/). It is very simple to use (drag and drop) and it even shows the location in Google Maps so you don't even have to convert the GPS location to decimal form :wink:.

### The photo I used for the example

I didn't reach to the top of Khakaborzi due to weather condition. And, was stuck in a small town name "Putao". One interesting fact about Putao is there were no theives. When it's time for the local market to close, the shopkeepers just leave their shop without locking the doors.

{{< figure
    src="putao.jpg"
    alt="Putao"
    default=true
    caption="Climbing Khakaborazi in 2019 (Kachin State, Myanmar)"
    >}}