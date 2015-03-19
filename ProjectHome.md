# About this project #

This project is a fork of [Django-stdimage](http://code.google.com/p/django-stdimage/), since the original project has not been updated since 2008 and none of the numerous patches sent by users have been integrated, I've decided to fork it and apply some of the patches myself.

This is intended principally for my usage, but I decided to host it publicly so it can be useful for everybody.

For the moment I don't plan to add new features, I will start by fixing long standing bugs and apply the patches that have been sent so far.

# Implemented features #

  * Rename files to a standardized name (using object id)
  * Resize images for that field
  * Automatically creates a thumbnail (resizing it)
  * Allow image deletion
  * Compatible with Django South _(new)_

# Usage examples #

It's not necessary to include in INSTALLED\_APPLICATIONS.

Import it in your project, and use in your models. Example:

```
[...]
from stdimage import StdImageField

class MyClass(models.Model):
    image1 = StdImageField(upload_to='path/to/img') # works as ImageField
    image2 = StdImageField(upload_to='path/to/img', blank=True) # can be deleted throwgh admin
    image3 = StdImageField(upload_to='path/to/img', size=(640, 480)) # resizes image to maximum size to fit a 640x480 area
    image4 = StdImageField(upload_to='path/to/img', size=(640, 480, True)) # resizes image to 640x480 croping if necessary
    image5 = StdImageField(upload_to='path/to/img', thumbnail_size=(100, 75)) # creates a thumbnail resized to maximum size to fit a 100x75 area
    image6 = StdImageField(upload_to='path/to/img', thumbnail_size=(100, 100, True)) # creates a thumbnail resized to 100x100 croping if necessary

    image_all = StdImageField(upload_to='path/to/img', blank=True, size=(640, 480), thumbnail_size=(100, 100, True)) # all previous features in one declaration
```
For using generated thumbnail in templates use "myimagefield.thumbnail". Example:
```
[...]
<a href="{{ object.myimage.url }}"><img alt="" src="{{ object.myimage.thumbnail.url }}"/></a>
[...]
```

Previous template syntax is new in release 7, because of the file storage refactoring, that changed the way that FileField attributes are accessed. For previous versions the syntax {{ object.myimage }} and {{ object.myimage\_thumbnail }} should be used.

# About image names #

StdImageField stores images in filesystem modifying its name. Renamed name is set using field name, and object primary key. Also it changes old windows "jpg" extesions to standard "jpeg".

Using image5 field previously defined (that creates a thumbnail), if an image called myimage.jpg is uploaded, then resulting images on filesystem would be (supose that this image belongs to a model with pk 14):

  * image5\_14.jpeg
  * image5\_14.thumbnail.jpeg