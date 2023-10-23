# odpmkd
A small set of tools to process OpenDocument Presentation files (ODP), and to transform them to markdown.
It will work acceptably for the "lecture notes style" slides, AKA "presentation-as-a-document" style: mostly text structured in bullet points. It will
not work well at all for more "presentation style" slides, where the visual aspect and the arrangement of elements
in the slides is important.

- `odptools` takes an odp file, and produces a cleaner version of that file.
- `odpmkd` takes an odp file and produces a markdown file trying to preserve its contents.

## Build
Read the ci.yml script for details on how to build.

If you don't need to build, you can download the latest wheel file from the releases section and install 
that with `pip install FILENAME.whl` (you can do that in a virtual environment or in your global python 
installation).

## Use examples
Take an odp file and remove all notes and hidden slides:
`odptools - input.odp -o input_cleaned.odp --remove_notes --remove_hidden` 

Take an odp file, and produce a markdown file of its contents including links to the images:
`odpmkd -i input.odp -x -m > input.md` 

## Notes
The resulting markdown will be better if the input odp file slides:

- Have a title, and some text structured in bullet points in the text area. This is the basic slide layout.
- Have a title and a single image.
- Emphasized words (bolds, underlines, italics) are processed and the emphasis is kept. 
    - However, LibreOffice may allow you to do some very weird things with this, that look good on screen but are not so good when you are parsing the slides. So this does not work fine some times.

Anything else might be processed, but the results will be worse:

- If the slide has several images, they will be kept, but their position and size on the original slide are lost, so the result will not be very good.
- Anything drawn using LibreOffice tools will be lost.
- Slides with just some text on them (often large text in the middle of the slide) will be processed, but that text will be the in same size as any other paragraph.
- Tables are lost.

The resulting markdown file has this structure:
 
- A level 1 title with the name of the document.  
- Level 2 titles with the title of the slides.
- Bullet points with the bullet points in the slides. These are nested following the nesting in the slides.

This structure is OK-ish for a presentation, but it is not so good if you look at the whole markdown as a document.

To improve the results, besides adapting your slides to the previous comments, you might try to transform
some slides where you have several visual elements properly arranged in the slide, or perhaps some tables, into 
a single image and put that in a new slide that will processed better by odpmkd. You can then hide the original 
slide in your  presentation, in case you want to make changes in the future, and keep the slide with the 
single image visible. 


## Copyright and License
&copy; Copyright 2023- Rubén Béjar <https://www.rubenbejar.com>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.

This is a derivate work, based on <https://github.com/seichter/odp2md>, &copy; 2019-2021 Hartmut Seichter, under the same license.