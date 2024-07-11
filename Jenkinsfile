
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Image Gallery</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        .folder-list, .gallery {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }
        .folder {
            padding: 10px;
            background-color: #f0f0f0;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .folder:hover {
            background-color: #e0e0e0;
        }
        .gallery img {
            width: 200px;
            height: 200px;
            object-fit: cover;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
            transition: transform 0.2s;
        }
        .gallery img:hover {
            transform: scale(1.05);
        }
        .full-image {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        .full-image img {
            max-width: 90%;
            max-height: 90%;
        }
        .close, .prev, .next {
            position: absolute;
            color: white;
            font-size: 2em;
            cursor: pointer;
        }
        .close {
            top: 20px;
            right: 20px;
        }
        .prev {
            left: 20px;
        }
        .next {
            right: 20px;
        }
        .thumbnails {
            display: flex;
            overflow-x: auto;
            padding: 10px;
            background-color: rgba(0, 0, 0, 0.5);
            width: 100%;
            justify-content: center;
        }
        .thumbnails img {
            width: 100px;
            height: 100px;
            object-fit: cover;
            margin: 0 5px;
            cursor: pointer;
            opacity: 0.6;
            transition: opacity 0.2s, border-color 0.2s;
            border: 2px solid transparent; /* Initially transparent border */
        }
        .thumbnails img:hover {
            opacity: 1;
        }
        .thumbnails img.active {
            border-color: white; /* White border for active thumbnail */
            opacity: 1;
        }
    </style>
</head>
<body>
    <h1>Image Gallery</h1>
    <h2><button><a href="http://192.168.178.122:8888">Back to Menu</a></button></h2>
    <div class="folder-list" id="folder-list"></div>
    <div class="gallery" id="gallery"></div>
    <div class="full-image" id="full-image">
        <span class="close" id="close">&times;</span>
        <span class="prev" id="prev">&#10094;</span>
        <img id="full-img" src="">
        <span class="next" id="next">&#10095;</span>
        <div class="thumbnails" id="thumbnails"></div>
    </div>

    <script>
        const parentFolder = 'MarcoDraw'; // Replace with your parent folder name
        const githubRepoUrl = 'https://api.github.com/repos/MarcoCoppola135/Gallery/contents/';  // Replace {username} and {repo} with your GitHub username and repository name
        let currentFolder = '';
        let images = [];
        let currentIndex = 0;

        async function fetchFolders() {
            const response = await fetch(`${githubRepoUrl}${parentFolder}`);
            const folders = await response.json();
            const folderList = document.getElementById('folder-list');
            folderList.innerHTML = '';
            folders.forEach(folder => {
                if (folder.type === 'dir') {
                    const folderElement = document.createElement('div');
                    folderElement.className = 'folder';
                    folderElement.textContent = folder.name;
                    folderElement.onclick = () => fetchImages(`${parentFolder}/${folder.name}`);
                    folderList.appendChild(folderElement);
                }
            });
        }

        async function fetchImages(folder) {
            currentFolder = folder;
            const response = await fetch(`${githubRepoUrl}${folder}`);
            images = await response.json();
            const gallery = document.getElementById('gallery');
            gallery.innerHTML = '';
            images.forEach((file, index) => {
                if (file.name.match(/\.(png|jpg|jpeg|gif)$/)) {
                    const imgElement = document.createElement('img');
                    imgElement.src = file.download_url;
                    imgElement.onclick = () => openImage(index);
                    gallery.appendChild(imgElement);
                }
            });
        }

        function openImage(index) {
            currentIndex = index;
            const fullImage = document.getElementById('full-image');
            const fullImg = document.getElementById('full-img');
            fullImg.src = images[currentIndex].download_url;
            fullImage.style.display = 'flex';
            updateThumbnails();
            document.addEventListener('keydown', handleKeydown);
        }

        function closeImage() {
            const fullImage = document.getElementById('full-image');
            fullImage.style.display = 'none';
            document.removeEventListener('keydown', handleKeydown);
        }

        function showNextImage() {
            currentIndex = (currentIndex + 1) % images.length;
            openImage(currentIndex);
        }

        function showPrevImage() {
            currentIndex = (currentIndex - 1 + images.length) % images.length;
            openImage(currentIndex);
        }

        function updateThumbnails() {
            const thumbnails = document.getElementById('thumbnails');
            thumbnails.innerHTML = '';
            images.forEach((file, index) => {
                if (file.name.match(/\.(png|jpg|jpeg|gif)$/)) {
                    const thumbElement = document.createElement('img');
                    thumbElement.src = file.download_url;
                    thumbElement.className = index === currentIndex ? 'active' : '';
                    thumbElement.onclick = () => openImage(index);
                    thumbnails.appendChild(thumbElement);
                }
            });
        }

        function handleKeydown(event) {
            if (event.key === 'ArrowRight') {
                showNextImage();
            } else if (event.key === 'ArrowLeft') {
                showPrevImage();
            }
        }

        document.getElementById('close').onclick = closeImage;
        document.getElementById('next').onclick = showNextImage;
        document.getElementById('prev').onclick = showPrevImage;

        fetchFolders();
    </script>
</body>
</html>
