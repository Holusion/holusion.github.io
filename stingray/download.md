---
layout: documentation
---

# Downloads
Download the latest commit from master branch to get updates. No stable release exists at the moment as Stingray is still in early development stage.

<center>
  <a href="https://github.com/Holusion/stingray/archive/master.zip">
    <button type="button" class="btn btn-default btn-lg">
      <span class="glyphicon glyphicon-info-sign" aria-hidden="true"></span> Download Release
    </button>
  </a>
</center>

<h1>Installation</h1>

<h4>dependencies</h4>

<p>
  Stingray requires SDL2 and libav/ffmpeg (libavcodec, libavformat, libavutil)
</p>
<p>
On debian / ubuntu :

{% highlight shell %}
apt-get install libavcodec-dev libavformat-dev libavutil-dev libsdl2-dev
{% endhighlight %}

<h4>Build</h4>
<p>
The usual should do :
</p>

{% highlight shell %}
./configure
make
sudo make install
{% endhighlight %}

stingray is then available as `/usr/bin/stingray`. The video conversion helper is in `/usr/bin/stingray-converter`.
