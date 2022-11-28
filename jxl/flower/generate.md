
  *How to generate the test set*

  Start with an image `flower.png`.
 - flower_alpha.png:
   ```
   convert flower.png -set colorspace Gray -separate -average grey.png
   convert flower.png grey.png -compose copy-opacity -composite flower_alpha.png
   rm grey.png
   ```
 - flower_cropped.jpg:
   ```
   convert flower.png -gravity center -crop 1040x1040+0+0 +repage flower_cropped.pnm
   cjpeg -outfile flower_cropped.jpg -quality 85 flower_cropped.pnm
   rm flower_cropped.pnm
   ```
 - flower.png.ffmpeg.y4m
    ```
    ffmpeg -i flower.png -pix_fmt yuvj420p flower.png.ffmpeg.y4m
    ```

 - flower.png.im_q85_*.jpg:
   ```
   convert flower.png flower.pnm
   convert flower.png -set colorspace Gray flower.pgm
   cjpeg -outfile flower.png.im_q85_420.jpg -sample 2x2 -quality 85 flower.pnm
   cjpeg -outfile flower.png.im_q85_420_progr.jpg -sample 2x2 -quality 85 -progressive flower.pnm
   cjpeg -outfile flower.png.im_q85_420_R13B.jpg -sample 2x2 -quality 85 -restart 13B flower.pnm
   cjpeg -outfile flower.png.im_q85_422.jpg -sample 2x1 -quality 85 flower.pnm
   cjpeg -outfile flower.png.im_q85_440.jpg -sample 1x2 -quality 85 flower.pnm
   cjpeg -outfile flower.png.im_q85_444.jpg -sample 1x1 -quality 85 flower.pnm
   cjpeg -outfile flower.png.im_q85_444_1x2.jpg -sample 1x2,1x2,1x2 -quality 85 flower.pnm
   cjpeg -outfile flower.png.im_q85_gray.jpg -quality 85 -grayscale flower.pgm
   cjpeg -outfile flower.png.im_q85_luma_subsample.jpg -sample 1x1,2x2,2x2 -quality 85 flower.pnm
   cjpeg -outfile flower.png.im_q85_rgb.jpg -sample 1x1 -quality 85 -rgb flower.pnm
   cjpeg -outfile flower.png.im_q85_rgb_subsample_blue.jpg -sample 2x2,2x2,1x1 -quality 85 -rgb flower.pnm
   ```

 - flower_small.*.{ppm,pgm,pam}
   ```
   convert flower.png -gravity center -crop 510x532+0+0 +repage flower_small.rgb.png
   convert flower_small.rgb.png -set colorspace Gray -separate -average flower_small.g.png
   convert flower_small.rgb.png flower_small.g.png -compose copy-opacity -composite flower_small.rgba.png
   convert flower_small.g.png flower_small.g.png -compose copy-opacity -composite flower_small.ga.png
   convert flower_small.rgb.png -ordered-dither o8x8 -depth 1 flower_small.rgb.depth1.ppm
   for i in `seq 2 16`; do convert flower_small.rgb.png -depth $i flower_small.rgb.depth$i.ppm; done
   convert flower_small.g.png -ordered-dither o8x8 -depth 1 flower_small.g.depth1.pgm
   for i in `seq 2 16`; do convert flower_small.g.png -depth $i flower_small.g.depth$i.pgm; done
   for i in `seq 2 16`; do convert flower_small.ga.png -depth $i flower_small.ga.depth$i.pam; done
   for i in `seq 2 16`; do convert flower_small.rgba.png -depth $i flower_small.rgba.depth$i.pam; done
   rm flower_small.*.png
   ```

 - flower_small.*.jpg
   ```
   cjpeg -outfile flower_small.q85_444_non_interleaved.jpg -scans non_interleaved_scan.txt -sample 1x1 -quality 85 flower_small.rgb.depth8.ppm
   cjpeg -outfile flower_small.q85_444_partially_interleaved.jpg -scans partially_interleaved_scan.txt -sample 1x1 -quality 85 flower_small.rgb.depth8.ppm
   cjpeg -outfile flower_small.q85_420_non_interleaved.jpg -scans non_interleaved_scan.txt -sample 2x2 -quality 85 flower_small.rgb.depth8.ppm
   cjpeg -outfile flower_small.q85_420_partially_interleaved.jpg -scans partially_interleaved_scan.txt -sample 2x2 -quality 85 flower_small.rgb.depth8.ppm
   ```
