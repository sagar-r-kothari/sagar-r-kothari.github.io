---
layout: post
title: "Play video in a UIView as a layer"
date: 2020-01-30 05:00:00 +0530
categories: Swift Video
---

```swift
import UIKit
import AVKit
import AVFoundation

extension YourViewController {
    func playVideo() {
        guard 
        	let path = Bundle.main.path(forResource: "Animation", ofType:"mov") 
        	else {
            	debugPrint("ConnectAnimation.mov not found")
            	return
        }
        player = AVPlayer(url: URL(fileURLWithPath: path))
        guard let player = player else { return }
        playerLayer = AVPlayerLayer(player: player)
        guard let playerLayer = playerLayer else { return }
        playerLayer.frame = videoContainerView.bounds
        videoContainerView.layer.addSublayer(playerLayer)
        player.actionAtItemEnd = .none
        player.play()
    }
}

```