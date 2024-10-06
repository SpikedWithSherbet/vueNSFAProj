#  Web Frameworks and Dynamic Data: Web App Development Rationale - u3260701

## Coding within Vue and Typescript

Coding with a new framework was a challenge that I found difficult in the beginning, and later became more proficient with
as the project had progressed. Creating the project with Vue had some minor issues with navigating the structure of the project, but eventually, it became less of an issue.

Coding within Typescript was quite difficult, and it took some effort to understand the small differences between syntax such as calling an item from a list or using `object.?[object]` for example.

## Using Dynamic Data

I wanted to use dynamic data in a unique way in comparison to what was discussed in class. I used a lot of `/title/:id` calls within my project to showcase specific items, and to get a range of a set with `/title-list/:ids`. Unfortunately, this ran into a few issues. Namely:

* Having to make an API call for each object would put unnecessary strain on the NFSAs servers, and most likely would get me forbidden from it being used in a professional project.
* `/title-list/:ids` automatically sorts the given results. This was an issue because I specifically ordered them in a manner that makes it correspond with the More Info images on the page. Therfore, titles for those items would not match the image.

Otherwise, I was able to get titles, summaries, and years from the collection itself, which was dynamic. 

## Non-dynamic parts

Unfortunately, there were many times in this project that I was not able to make elements I wanted to be dynamic. 

First, all of the images on the page are not media from the query results themselves, but thumbnails of videos from the already
existing NSFA page pertaining to TV Game Shows (https://www.nfsa.gov.au/collection/curated/australian-tv-game-shows-1950s-now). This was because all of the data that I was working with did not have any media I could use, and I wasn't able to ask for access from the NFSA in time.

I also ordered every item in the specific item lists manually. This was extremely tedious. Because I could not pick between a range of years using `fromYear` at the end of the query, I had to do this by hand. 

Other parts did not need to by dynamic, and therefore did not require much effort.

## Use of AI in the project

I used ChatGPT for syntax/logic errors, and some confusing elements like adding pagination to my list of items. I felt like using it in this project was necessary, as I am new to Typescript and there were few times I could ask for guidance from a colleague. 

I explicitly explain where I have used it in the project in my code. I also explain what the code does to eliminate any confusion.

## What I learned

Other than the basic concepts of the Vue framework and using the API, I also learned a few interesting commands through experimenting, and AI helping with some syntax.

1. Computed, which allows functions to update variables in real time without having to call the function each time. Helpful for pagination.
2. Mounting, which performs the function when the Vue component is mounted to the DOM tree.
3. `Slice` which clones an array, and selects the range of numbers from its start to its end.
4. `Math.ceil` which rounds up and integers to the closest one.

Other than that I used this CRT TV effect here (https://medium.com/@dovid11564/using-css-animations-to-mimic-the-look-of-a-crt-monitor-3919de3318e2)

## Other Challenges

In comparison to the prototype, I wanted to use filters and keywords to sort the different info. This would have been possible with enough time. Unfortunately, I ran out of time to implement this feature, and decided to use the more info to navigate to the NFSA's collection page of the particular item. 

I also didn't use other Vue components other than HelloWorld.vue. I also didn't want to change any of the names of the files if it broke anything crucial. 

## Moving Forward

In the next phase of this project, I want to go through some User Testing to make the UX of the project better, attempt to implement some of the missing concepts from the prototype, and to re-iterate on some elements I had to compromise on.

## References

National Film and Sound Archive. (2021, September 20). Australian TV Game Shows from the 1950s to Now. Www.nfsa.gov.au. (https://www.nfsa.gov.au/collection/curated/australian-tv-game-shows-1950s-now)

OpenAI. (2024, October 1). ChatGPT . ChatGPT; OpenAI. (https://chatgpt.com/)

Dovid Edelkopf. (2023, January 24). Using CSS Animations To Mimic The Look Of A CRT Monitor. Medium. (https://medium.com/@dovid11564/using-css-animations-to-mimic-the-look-of-a-crt-monitor-3919de3318e2)

Down Arrow by th studio on flaticon: (https://www.flaticon.com/free-icon/down-arrow_2985150?term=down+arrow&page=1&position=1&origin=search&related_id=2985150)

DSEG Seven Segment Font by keshikan on GitHub (https://github.com/keshikan/DSEG/releases)


