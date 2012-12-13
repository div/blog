---
title: Process specific versions of your carrierwave uploads in the background
date: '2012-12-13'
description:
categories:
---

I needed to selectively process some versions of an uploaded file in the background job. But wanted also to process some small preview (let's say we are uploading images and it is a :thumb) synchronously and show it to the user as soon as it is done. Carrierwave allowed us only to recreate all versions at once, so i had to fix it first. Since this [Pull](https://github.com/jnicklas/carrierwave/pull/906) has been merged in, you can pass versions to recreate_versions! method like so:

```
recreate_versions!(:thumb, :small)
```

Now to be able to process selected versions in the background job you only need some guard to not process those versions synchronously:

```
class MyUploader < CarrierWave::Uploader::Base
# some stuff omitted

  version :thumb do
    process :process_thumb
  end

  version :large, if: :process_async? do
    process :process_large
  end

  def process_async?(image = nil)
    !! @process_async
  end

  def process_async=(value)
    @process_async = value
  end
end
```

And in your worker you just set @process_async to true:

```
class MyWorker
  def self.perform(image_id)
    image = Image.find(image_id)
    uploader = image.file
    uploader.process_async = true
    uploader.recreate_versions!(:large)
    uploader.process_async = false
  end
end
```

To trigger your worker add this to the the model:

```
after_commit :queue_processing, if: :file_previously_changed?

private

  def file_previously_changed?
    previous_changes[:file].present?
  end

  def queue_processing
    Resque.enqueue(MyWorker, self.id)
  end
```

And that's it!
