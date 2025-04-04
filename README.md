# -Identify-the-Blue-Colored-Object-in-Cluttered-Image
% Read the image
image = imread('peacock.jpg');

% Convert the image from RGB to HSV color space
hsvImage = rgb2hsv(image);

% Define thresholds for 'Hue', 'Saturation' and 'Value' to isolate blue color
hueThresholdLow = 0.55; % Adjust these values based on your image
hueThresholdHigh = 0.75;
saturationThresholdLow = 0.4;
saturationThresholdHigh = 1.0;
valueThresholdLow = 0.2;
valueThresholdHigh = 1.0;

% Create a binary mask based on the thresholds
blueMask = (hsvImage(:,:,1) >= hueThresholdLow) & (hsvImage(:,:,1) <= hueThresholdHigh) & ...
           (hsvImage(:,:,2) >= saturationThresholdLow) & (hsvImage(:,:,2) <= saturationThresholdHigh) & ...
           (hsvImage(:,:,3) >= valueThresholdLow) & (hsvImage(:,:,3) <= valueThresholdHigh);

% Apply the mask to the original image
blueObjects = bsxfun(@times, image, cast(blueMask, 'like', image));

% Display the original image and the blue objects
figure;
subplot(1, 2, 1);
imshow(image);
title('Original Image');

subplot(1, 2, 2);
imshow(blueObjects);
title('Blue Objects');

% Optionally, you can use morphological operations to clean up the mask
blueMask = imopen(blueMask, strel('disk', 5)); % Remove small objects
blueMask = imclose(blueMask, strel('disk', 5)); % Fill small holes

% Display the cleaned mask
figure;
imshow(blueMask);
title('Cleaned Blue Mask');
