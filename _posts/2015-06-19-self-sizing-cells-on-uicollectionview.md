---
layout: post
title: Self Sizing Cells on UICollectionView
date: 2015-06-19
---

My goal was to do a home page with layout like Pinterest.
<p class="p1"><a href="http://anilgoktas.github.io/assets/2015/06/Screen-Shot-2015-06-19-at-8.55.25-AM.png"><img class=" size-medium wp-image-95 aligncenter" src="http://anilgoktas.github.io/assets/2015/06/Screen-Shot-2015-06-19-at-8.55.25-AM-206x300.png" alt="Screen Shot 2015-06-19 at 8.55.25 AM" width="206" height="300" /></a></p>
<p class="p1" style="text-align: left;">After some research I found this projects; <a href="https://github.com/xhzengAIB/PinterestAnimator">PinterestAnimator</a> and <a href="https://github.com/demon1105/PinterestSwift">PinterestSwift</a>. These really helped me about collection view layout, especially <a href="https://github.com/chiahsien/CHTCollectionViewWaterfallLayout">CHTCollectionViewWaterfallLayout</a>. However there is still one problem called self-sizing.</p>
<p class="p1" style="text-align: left;">So I searched more and found this fantastic extension on <a href="https://github.com/Alamofire/Alamofire">Alamofire</a> in order to download images, thanks to <a href="http://www.raywenderlich.com/85080/beginning-alamofire-tutorial">Ray Wenderlich</a>.</p>

<pre><code>
    extension Alamofire.Request {
        class func imageResponseSerializer() -&gt; Serializer {
            return { request, response, data in
                if let data = data {
                    let image = UIImage(data: data)
                    return (image, nil)
                }
                return (nil, nil)
            }
        }    

        func responseImage(completionHandler: (NSURLRequest, NSHTTPURLResponse?, UIImage?, NSError?) -&gt; Void) -&gt; Self {
            return response(serializer: Request.imageResponseSerializer(), completionHandler: {
                (request, response, image, error) in
                completionHandler(request, response, image as? UIImage, error)
            })
        }
    }
</code></pre>
<p class="p1"><span class="s1">Image size is accessible once it's downloaded but we know its width will be (view.size-3*inset)/columnNumber. Therefore we can filter image to that size in order to achieve faster loading time. Here is the code:</span></p>

<pre><code>
    let item = items[indexPath.row]
            
    // Prepare for re-use
    cell.request?.cancel()
    cell.imageView.image = nil // Caching can be implemented here
            
    let imageScale = UIScreen.mainScreen().scale
    let imagePixel = Int(cellWidth * imageScale)
    let filterURLString = "http://cdn.filter.to/\(imagePixel)x\(imagePixel)/"
    // We should delete "http://" of imageURLString in order to use filtering
            
    if
    let imageURLString = item.imageURLString,
    let rangeOfURLString = imageURLString.rangeOfString("//", options: .allZeros, range: nil, locale: nil),
    let imageURL = NSURL(string: filterURLString+imageURLString.substringFromIndex(advance(rangeOfURLString.startIndex,2)))
    {
        cell.request = Alamofire.request(.GET, imageURL, parameters: nil, encoding: .URL).responseImage({
            (_, _, image, error) -&gt; Void in
            if let image = image where error == nil {
                // Resize cell
                cell.imageView.image = image
                cell.title = item.title
                cell.subtitle = item.subtitle
                let imageViewHeight = self.cellWidth * image.ratio
                let extraHeight = cell.titleLabel.frame.height + cell.subtitleLabel.frame.height
                item.estimatedCellSize = CGSize(width: self.cellWidth, height: imageViewHeight + extraHeight)
                // Perform updates
                collectionView.performBatchUpdates(nil, completion: nil)
            }
        })
    }
</code></pre>
<p style="text-align: center;"><span class="s1">Result:
<a href="http://anilgoktas.github.io/assets/2015/06/iOS-Simulator-Screen-Shot-Jun-19-2015-1.57.15-PM.png"><img class=" size-medium wp-image-118 aligncenter" src="http://anilgoktas.github.io/assets/2015/06/iOS-Simulator-Screen-Shot-Jun-19-2015-1.57.15-PM-169x300.png" alt="iOS Simulator Screen Shot Jun 19, 2015, 1.57.15 PM" width="169" height="300" /></a></span></p>
&nbsp;

You can find the project <a href="https://github.com/anilgoktas/SelfSizingCollectionViewCell">here</a>.