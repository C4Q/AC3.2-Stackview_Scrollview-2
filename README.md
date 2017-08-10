# AC3.2 Stackview and Scrollview: Part II

---
### Readings

1. [`UIStackView` - Apple](https://developer.apple.com/reference/uikit/uistackview)
2. [AutoLayout Guide: Stack Views - Apple](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/AutolayoutPG/LayoutUsingStackViews.html)
3. [Into to Stack Views in iOS 9 - App Coda](http://www.appcoda.com/stack-views-intro/)
4. [UIStackView by Example - Hacking With Swift](https://www.hackingwithswift.com/read/31/2/uistackview-by-example)

#### References

1. [`UIViewContentMode` - Apple](https://developer.apple.com/documentation/uikit/uiviewcontentmode)

---
### Vocabulary


---
### 0. Objectives

1. Exploring the `UIStackView` UI element for quickly aligning rows and/or columns of known content
2. Further reinforcing the use of `UIScrollView` to display large amounts of content in a limited space
3. Practicing layout with a storyboard

---
### 1. Intro

Hopefully, the [previous lesson exercises](https://github.com/C4Q/AC3.2-Stackview_Scrollview-1#exercises-️️-️️) have shown you that it isn't an entirely trivial task to work with scroll views. And yet, they're everywhere. A lot of this effort needed is mitigated by using `UITableView` and `UICollectionView` to neatly arrange elements, but sometimes it doesn't make sense to implement either of those for very simple setups that require some degree of dynamic sizing and updating.

That's one of the reasons Apple introduced the `UIStackView`: to streamline the vertical and horizontal layouts of a known number of views. It handles a good portion of autolayout for you, which allows you to create a UI that adapts to screen size and layout changes much more easily than if you were coding a `UIScrollView`. Note however, `UIStackView` doesn't handle all of the autolayout which is why it's still critical knowledge (and when we get into programmatic autolayout later in the course, this will be even further emphasized). A deep understanding of autolayout and stack views will allow you to create some pretty impressive and responsive designs without *much* work.

Let's start simple though: We're going to take a basic look at a stack view to create a horizontally scrolling row of Pokémon icons.

---
### 2. PokéStack

Today's lesson is going to focus on implementing a simple, horizontally scrolling stack view. We're going to take a look at how to embed elements into a stack view, along with some of the options we have for configuring how elements will be laid out once they are embedded in the stack view.

1. Drag in a new `UIViewController` into `Main.storyboard` and set it to be the initial view controller
2. With the view controller selected, go into `Editor > Embed In > Tab Bar`
3. Drag in four `UIImageView` and set their images to the four starter Pokémon. For the unfamiliar, they are: `Pikachu, Squirtle, Bulbasaur` and `Charmander`.
4. By default, images added will have their `contentMode` set to `Aspect Fill`. `Aspect Fill` will attempt to fill the bounds of the view while preserving the aspect ratio of the content. Two other common options are `scale fill` which will attempt to fill the bounds of the view stretching content if needed, and `aspect fit` which will size the content to fit within the bounds of the view while preserving aspect ratio. See the table below for further explanation (and feel free to play around with the other content modes to get a sense for each of them):

<table>
	<thead>
		<tr>
			<td>Image Views</td>
			<td>Scale Fill</td>
			<td>Aspect Fill</td>
			<td>Aspect Fit</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><img src="./Images/adding_image_views.png" width="180" alt="Center Aligning ImageViews"></td>
			<td><img src="./Images/scale_to_fill.png" width="180" alt="Scale to Fill"></td>
			<td><img src="./Images/aspect_fill.png" width="180" alt="Aspect Fill"></td>
			<td><img src="./Images/aspect_fit.png" width="180" alt="Aspect Fit"></td>
		</tr>
	</tbody>
</table>


5. When dealing with images, we're usually most concerned about preserving aspect ratio (so that the image doesn't appear distorted). Change the image content mode to `Aspect Fit`. Also, set the background color of the image views so that we can better see the bounds of the view.

<table>
	<thead>
		<tr>
			<td>Aspect Fit, Bkdg Color</td>
			<td>Util Panel, Content Mode</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td><img src="./Images/vertical_fit_pokestack.png" width="250" alt="Aspect Fitted with background color"></td>
			<td><img src="./Images/aspect_fit_util_panel.png" width="250" alt="Utilities Panel, Selecting Content Mode"></td>
		</tr>
	</tbody>
</table>

6. Now that we have the four imageviews set up we can add them to a stack view. There are a couples of ways to add a stack view in storyboard:
	- You can drag in either a `Horizontal Stack View` or `Vertical Stack View` from the Object Library in the right pane
	- You can select the views you'd like to place in a stack view, and then go to `Editor > Embed In > Stack View`
	- Or, you can select the views and click on the `Stack` button, conveniently located next to the `Align`, `Pin` and `Resolve AutoLayout Issues` buttons on the bottom right corner of Interface Builder.
	- <img src="./Images/embed_in_stack_button.png" alt="Convenient Embedding Stack Button">
7. There are three main configuration options needed to consider for a stack view (see `UIStackView` documentation under ["Managing the Stack View's Apperance"](https://developer.apple.com/documentation/uikit/uistackview))
	- `Axis` - the orientation of the stack, `vertical` or `horizontal`
	- `Alignment` - the layout of the arranged views *perpendicular* to the stack’s axis
	- `Distribution` - the layout of the arranged views *along* the stack’s axis

> Developer's Note: Once again, it's encouraged that you try out variations of each of these properties to get a sense for what each is accomplishing. Visual changes made by each of these properties is best understood having used them for a while.

You may have already noticed how each of these options affects the contents of the stack view.

<table border-width="0"><thead></thead>
	<tbody>
		<tr>
			<td>Though one thing you might not immediately realize is that the four icons have a maximum height and width of `128pt`. But each one has a `width` or `height` slightly smaller than `128pt`. For exaple, the image of Squirtle is slightly narrower than the other icons. You can observe this by switching the `alignment` of the stack view to be `center` instead of `fill`.</td>
			<td width="300"><img src="./Images/alignment_center_option.png" alt="Slightly narrow icons"></td>
		</tr>
	</tbody>
</table>

<br>

<br>

<br>
<img src="./Images/slightly_narrower_pokemon.png" width="400" alt="Slightly narrower Squirtle">
<br>

Meaning, in a `Horizontal` layout with `Fill` alignment and distribution you will be able to see that the `UIImageView` becomes a bit narrower. Using these slight differences, these 4 icons are a good way to visually identify the results of changes the different options of the stack view.
![Perfect Padding](http://imgur.com/BPfbJxBl.jpg)







8. Let's make this particular stack view
	-`Axis` = `Horizontal`
	- `Alignment` = `Fill`
	- `Distribution` = `Fill Equally`



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
