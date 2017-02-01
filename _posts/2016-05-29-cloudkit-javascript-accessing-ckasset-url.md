---
layout: post
title: CloudKit JavaScript accessing CKAsset URL
date: 2016-05-29
---

You can use CloudKit JavaScript for accessing url of asset. Here is an example;

(Used <a href="https://github.com/Alamofire/Alamofire">Alamofire</a> and <a href="https://github.com/SwiftyJSON/SwiftyJSON">SwiftyJSON</a>)

<pre><code>
    func testRecordRequest() -&gt; Request {
        let urlString = "https://api.apple-cloudkit.com/database/1/" + Constants.container + "/development/public/records/query?ckAPIToken=" + Constants.cloudKitAPIToken
        let query = ["recordType": "TestRecord"]
        return Alamofire.request(.POST, urlString, parameters: ["query": query], encoding: .JSON, headers: nil)
    }
</code></pre>

JSON response contains a "downloadURL" for the asset.

<pre><code>
    "downloadURL": "https://cvws.icloud-content.com/B/.../${f}?o=AmVtU..."
</code></pre>

"${f}" seems like a variable so change it to anything you like.

<pre><code>
    let downloadURLString = json["fields"][Keys.image]["value"]["downloadURL"].stringValue
    let recordName = json["recordName"].stringValue
    let fileName = recordName + ".jpg"
    let imageURLString = downloadURLString.stringByReplacingOccurrencesOfString("${f}", withString: fileName)
</code></pre>

And now we can use this urlString to create a NSURL and use it with any image&amp;cache solutions such as Kingfisher, HanekeSwift etc.
(You may also want to save image type png/jpg)