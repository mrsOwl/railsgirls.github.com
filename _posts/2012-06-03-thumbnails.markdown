---
layout: default
title: Мініатюри для списку ідей
permalink: thumbnails
---

# Створення мініатюр із Carrierwave

*Created by Miha Filej, [@mfilej](https://twitter.com/mfilej)*

__Наставник__: Поясни, що робить визначення ширини картинки в HTML в кінці 4 пункту,
і чим воно відрізняється від зміни розміру картинок на сервері.

## *1.*Інсталяція ImageMagick

* Linux: На Ubuntu і Debian, виконай `sudo apt-get update`, `sudo apt-get install imagemagick`. Потiм вiдповiсти "так" (y & enter).

__Наставник__: Що таке ImageMagick і як він відрізняється від бібліотек/гемів, котрі ми використовували раніше?

Відкрий `Gemfile` в проекті і додай

{% highlight ruby %}
gem 'mini_magick', '3.8.0'
{% endhighlight %}

під стрічкою

{% highlight ruby %}
gem 'carrierwave'
{% endhighlight %}

У терміналі виконай bundle, та перезевантаж робоче середовище:

{% highlight sh %}
bundle
{% endhighlight %}

## *2.*Скажемо нашій аплікації, щоб створювала мініатюри, коли ми завантажуємо зображення

Відкрий `app/uploaders/picture_uploader.rb` і знайди таку стрічку:

{% highlight ruby %}
  # include CarrierWave::MiniMagick
{% endhighlight %}

Видали цей `#` знак.

__Наставник__: Поясни ідею коментарів у коді.

Під стрічкою, яку щойно змінили, додай:

{% highlight ruby %}
version :thumb do
  process :resize_to_fill => [50, 50]
end
{% endhighlight %}

Відтепер завантажені зображення змінюватимуть свій розмір, але ті, які ми уже маємо,
не зазнають змін.
Тож відредагуй якусь ідею, ще раз додавши зображення.

## *3.*Відображення мініатюр

Щоб перевірити, чи завантажене зображення змінило свій розмір, відкрий
`app/views/ideas/index.html.erb`. Заміни стрічку

{% highlight erb %}
<%= image_tag idea.picture_url, width: '100%' if idea.picture.present? %>
{% endhighlight %}

на

{% highlight erb %}
<%= image_tag idea.picture_url(:thumb) if idea.picture.present? %>
{% endhighlight %}

Подивися на список ідей у браузері, щоб побачити чи є там мініатюри.
