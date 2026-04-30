
As an exercise, how much memory will a 800 × 600 pixel bitmap require?
800 × 600 × 4 bytes = 1,920,000 bytes ≈ 1.83 MB

 then it should be transferred with the minimum number of bytes. Always apply the best compression method for each asset.
The size of text-based assets, such as HTML, CSS, and JavaScript, can be reduced by 60%–80% on average when compressed with Gzip

Images, on the other hand, require a more nuanced consideration:

- Images account for over half the transferred bytes of an average page.
- mage files can be made smaller by eliminating unnecessary metadata.
- mages should be resized on the server to avoid shipping unnecessary bytes.
- An optimal image format should be chosen based on type of image.
- Lossy compression should be used whenever possible.

• WebP lossy images are 25%–34% smaller in size compared with JPEGs