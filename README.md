# AC3.2 Stackview and Scrollview: Part II

---
### Readings

1. [`UIStackView` - Apple](https://developer.apple.com/reference/uikit/uistackview)
2. [AutoLayout Guide: Stack Views - Apple](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/LayoutUsingStackViews.html)
3. [Into to Stack Views in iOS 9 - App Coda](http://www.appcoda.com/stack-views-intro/)
4. [UIStackView by Example - Hacking With Swift](https://www.hackingwithswift.com/read/31/2/uistackview-by-example)

---
### Vocabulary


---
### 0. Objectives

1. Exploring the `UIStackView` UI element for quickly aligning rows and/or columns of known content
2. Further reinforcing the use of `UIScrollView` to display large amounts of content in a limited space



---
### 1. Intro

Hopefully, the [previous lesson exercises](https://github.com/C4Q/AC3.2-Stackview_Scrollview-1#exercises-️️-️️) have shown you that it isn't an entirely trivial task to work with scroll views. And yet, they're everywhere. A lot of this effort needed is mitigated by using `UITableView` and `UICollectionView` to neatly arrange elements, but sometimes it doesn't make sense to implement either of those for very simple setups that require some degree of dynamic sizing and updating.

That's one of the reasons Apple introduced the `UIStackView`: to streamline the vertical and horizontal layouts of a known number of views. It handles a good portion of autolayout for you, which allows you to create a UI that adapts to screen size and layout changes much more easily than if you were coding a `UIScrollView`. Note however, `UIStackView` doesn't handle all of the autolayout which is why it's still critical knowledge (and when we get into programmatic autolayout later in the course, this will be even further emphasized). A deep understanding of autolayout and stack views will allow you to create some pretty impressive and responsive designs without *much* work.

Let's start simple though: We're going to take a basic look at a stack view to create a horizontally scrolling row of Pokémon icons.

---

### 2. PokéStack
1. Drag in a new `UIViewController` into `Main.storyboard` and set it to be the initial view controller
2. With the view controller selected, go into `Editor > Embed In > Tab Bar`

2. Drag in 4 `UIImageView` and set their images to the 4 starter Pokémon (yes, Pikachu counts as a starter).
  - Set the background color of the image views to whatever you prefer, so long as it stands out on a white background
  - Change the image content mode to `Aspect Fit`. This resizing mode keeps the original image's aspect ratio and scales the image to fit inside the frame of the `UIImageView` (feel free to play around with the other content modes later to get a sense for each of them)
  - ![Just Added](http://imgur.com/dHFwYqUm.jpg)
3. Add the 4 image views to a stack view. There are a lot of ways to add a stack view to IB
  - You can drag in either a `Horizontal Stack View` or `Vertical Stack View` from the Object Library in the right pane
  - You can select the views you'd like to place in a stack view, and then go to Editor > Embed In.. > Stack View
  - Or, you can select the views and click on the "Stack" button, conveniently located next to the "Align", "Pin" and "Resolve AutoLayout Issues" buttons on the bottom right corner of IB.
  - ![Vertically Stacked](http://imgur.com/cQzDXeEm.jpg)
4. There are three major options to configure a stack view (refer to the Apple doc for full explanations)
  - `Axis`
  - `Alignment`
  - `Distribution`
5. Let's make this particular stack view
  - `Axis` = `Horizontal`
  - `Alignment` = `Fill`
  - `Distribution` = `Fill Equally`

You already probably noticed how each of these options affects the contents of the stack view. What's rather nice is that the 4 icons, collectively, have a maximum height and width of `128pt`. But on their own, they have `width` or `height` slightly smaller than `128`.
![Squirtle Squirt](http://imgur.com/PFG4nrAm.jpg)
For example, the Squirtle icon has a width of slightly less than `128`, so in a `Horizontal` layout with `Fill` aligment and distribution you will be able to see that the `UIImageView` becomes a bit narrower. Using these slight differences, these 4 icons are a good way to visually identify the results of changes the different options of the stack view.
![Perfect Padding](http://imgur.com/BPfbJxBl.jpg)

1. Now just add the following constraints to the stackview:
  - `8pt` left and top margins
2. Run the project...
  - ![Needs Scrolling](http://imgur.com/VFYTt0Bm.jpg)
  - Ah.. bummer. I guess we'll need a scroll view
3. Select the stack view, then go to Editor > Embed In > Scroll View
4. For the scroll view, you'll need the following constraints:
  - Pin to top, left and right edges of its super view
5. For the stack view, add:
  - `8pt` top, left, right, bottom margins
  - Center Vertically In Container
6. Run the project

![Final Scrolling](http://imgur.com/g8bI1gzl.jpg)

What's nice about using the image views (along with the stack view) is that they define their own intrinsic content size. So we have far fewer constraints needed to satisfy autolayout.

![Constraints](http://imgur.com/D01DJ7Ql.jpg)

---
### 3. Exercises

1. Continue on and create 2 more horizontally scrolling stack views using the other two pokemon asset categories in `Assets.xcassets` ("Common Pokemon", "Uncommon Pokemon")
2. Add a label just above each stackview with that group's category
3. Select all of these views (the 3 stack views and 3 labels) and embed them in a vertical stack view
4. Now, with that vertical stack view selected, embed everything in a vertical-only scroll view.
5. (extra) If you have time and are interested, take a look at the [AutoLayout Guide: Stack Views](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/LayoutUsingStackViews.html) link. Experiment with different layouts and see what you can create. Just be sure that your storyboard doesn't list any warnings or errors.
