<script>
  import { onMount } from 'svelte';

  let image;
  let isDragging = false;
  let startX, startY, currentX, currentY;

  const handleMouseDown = (event) => {
    isDragging = true;
    startX = event.clientX;
    startY = event.clientY;
    currentX = startX;
    currentY = startY;
  };

  const handleMouseUp = () => {
    isDragging = false;
  };

  const handleMouseMove = (event) => {
    if (isDragging) {
      const deltaX = event.clientX - currentX;
      const deltaY = event.clientY - currentY;
      const left = parseInt(image.style.left || '0');
      const top = parseInt(image.style.top || '0');
      image.style.left = `${left + deltaX}px`;
      image.style.top = `${top + deltaY}px`;
      currentX = event.clientX;
      currentY = event.clientY;
    }
  };

  const uploadImage = () => {
    const input = document.createElement('input');
    input.type = 'file';
    input.accept = 'image/*';
    input.addEventListener('change', (event) => {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.addEventListener('load', () => {
        image.src = reader.result;
      });
      reader.readAsDataURL(file);
    });
    input.click();
  };

  const deleteImage = () => {
    image.src = '';
    image.style.left = '0';
    image.style.top = '0';
  };

  onMount(() => {
    image = document.getElementById('profile-image');
  });
</script>

<style>
  .card-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .card {
    width: 250px;
    height: 250px;
    background-color: #F5F5F5;
    border-radius: 50%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    position: relative;
  }

  #profile-image {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    object-fit: cover;
    cursor: move;
    position: absolute;
  }

  .edit-options {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 1rem;
  }

  .edit-options button {
    background-color: #FFFFFF;
    border: none;
    color: #333333;
    padding: 0.5rem;
    display: flex;
    border-radius: 5px;
    cursor: pointer;
  }

  .card:hover #profile-image {
    filter: brightness(70%);
  }

  .card:hover + .edit-options {
    display: flex;
  }
</style>

<div class="card-container">
  <div class="card">
    <img id="profile-image" src="" alt="Profile" on:mousedown={handleMouseDown} on:mouseup={handleMouseUp} on:mousemove={handleMouseMove}>
  </div>
  <div class="edit-options">
    <button on:click={uploadImage}>Upload Image</button>
    <button on:click={deleteImage}>Cancel</button>
  </div>
</div>
