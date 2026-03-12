<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/850afb70-4529-4893-9c15-228267cd407f" />

# AFFILIATE-MARKETTER-AUTOMATION
This project is basically an automated content engine built with n8n. The idea behind it came from a simple problem that many content pages face. Posting every day on multiple platforms takes time and the work is mostly repetitive. Downloading posts, rewriting captions, uploading files, and then publishing again on different platforms becomes a routine task that slowly eats hours.

So this workflow tries to remove most of that manual work.

Instead of a human checking pages all the time, the system listens for new content automatically. When something new appears, the workflow wakes up, processes the post, prepares it for distribution, and then publishes it where needed. Once it is running properly it can continue doing this without much supervision.

The entire system is built around n8n acting like the central brain that coordinates everything.

The automation begins with the Apify Instagram scraper. Apify periodically scans the source account and collects the latest posts. When a scraper run finishes successfully, the workflow is triggered. At that moment the dataset containing the scraped posts is retrieved.

But the workflow does not blindly process everything. Instead it sorts the posts using timestamps and selects the newest one. This prevents duplicates and ensures the system always focuses on fresh content.

Once the newest post is identified, the next step is understanding what type of content it actually is. Instagram posts can be a little tricky because they do not always follow the same structure. Sometimes there is only a single image. Sometimes it is a video. Other times it is a carousel containing multiple images or even mixed media.

The normalization stage exists for this reason. It analyzes the raw post data and converts it into a structured format that the rest of the system can understand. During this stage important fields are extracted such as the caption text, image URLs, video URLs, timestamps, and any child media items.

After the structure is clear, the caption moves into the AI stage.

The workflow uses a local language model running through Ollama. In this case the Qwen model is used. The job of this model is not to generate long paragraphs but to reshape the caption into something very short and more aligned with meme style posts. Mentions and usernames are removed, long sentences are trimmed, and sometimes the caption is replaced entirely with a short phrase. The goal is simply to produce something quick and engaging rather than descriptive.

Once the caption is ready, the media asset is prepared for publishing.

Instead of directly sending the original media to platforms, the file is uploaded to Cloudinary first. Cloudinary works as a storage and delivery layer for the workflow. This step gives the media a stable URL and ensures it can be accessed reliably by multiple APIs. It also reduces the chances of failures that can occur when platforms try to fetch media directly from temporary sources.

At this stage the workflow has two important things ready: a processed caption and a hosted media file.

Now the routing logic begins.

The workflow checks the structure of the post and decides which publishing pipeline it should follow. Videos go through a video distribution route where they can be uploaded to platforms like YouTube and also used for Instagram Reels. Before publishing, the system downloads the binary video file so the APIs receive the correct format.

If the post contains a single image, the process becomes simpler. The image URL and generated caption are sent directly to the Instagram publishing endpoint.

Carousel posts require a slightly more complex approach. Because a carousel contains multiple media items, the workflow splits the post into separate child elements. Each element is uploaded individually and then collected again using an aggregation step. After that the Facebook Graph API is used to rebuild the items into a proper carousel post so that the final result appears correctly on Instagram.

To keep everything stable, the workflow includes small delay steps between publishing actions. These short waits allow platforms enough time to process uploaded media containers before the next request is made. Without these waits many APIs tend to fail because the media has not finished processing yet.

When all steps are complete, the content ends up published across multiple platforms automatically. What originally started as a single source post becomes a distributed piece of content across different channels.

In practice this turns the workflow into something like a small automated content factory.

One source of content goes in.
AI adjusts the caption.
Media gets prepared and stored.
The system routes everything to the correct platforms.
And the posts appear automatically.

For creators or affiliate marketers managing several pages, systems like this can save a lot of time. Instead of spending hours every day repeating the same uploading steps, the automation handles the routine work in the background.

The project is also a good example of how different tools can be combined into a single automation pipeline. Scraping, AI caption processing, media hosting, and multi-platform publishing are all connected together inside one workflow.

It is not meant to replace human creativity or content strategy, but it shows how automation can remove the mechanical parts of running social media pages. Once the boring steps are handled by the system, the focus can shift more toward choosing good sources and planning growth rather than doing repetitive posting tasks every day.
