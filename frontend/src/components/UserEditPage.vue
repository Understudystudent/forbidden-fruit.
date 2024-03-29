<template>
    <div class="container mt-5">
      <h1 class="mb-4 text-center text-black display-2 font-italic cursive">Edit Your Account</h1>
      <form @submit.prevent="updateUser" class="row">
        <!-- User Image -->
        <div class="col-md-4 mb-3">
          <label for="userImage" class="text-center text-black">Profile Image:</label>
          <input type="file" class="form-control-file" id="userImage" name="userImage" accept="image/*"
            @change="handleImageUpload">
          <img :src="imageUrl" alt="User Image" class="img-fluid mt-2">
          <p class="text-black">Image size cannot exceed 50kb.</p>
        </div>
        <div class="col-md-8">
          <!-- First Name -->
          <div class="form-group">
            <label for="firstName" class="text-black">First Name:</label>
            <input v-model="formData.firstName" type="text" class="form-control" id="firstName" name="firstName">
          </div>
          <!-- Last Name -->
          <div class="form-group">
            <label for="lastName" class="text-black">Last Name:</label>
            <input v-model="formData.lastName" type="text" class="form-control" id="lastName" name="lastName">
          </div>
          <!-- Age -->
          <div class="form-group">
            <label for="userAge" class="text-black">Age:</label>
            <input v-model.number="formData.userAge" type="number" class="form-control" id="userAge" name="userAge">
          </div>
          <!-- Gender -->
          <div class="form-group">
            <label for="gender" class="text-black">Gender:</label>
            <select v-model="formData.gender" class="form-control" id="gender" name="gender">
              <option value="Male">Male</option>
              <option value="Female">Female</option>
              <option value="Other">Other</option>
            </select>
          </div>
          <!-- Phone Number -->
          <div class="form-group">
            <label for="phoneNumber" class="text-black">Phone Number:</label>
            <input v-model="formData.phoneNumber" type="number" class="form-control" id="phoneNumber" name="phoneNumber"
              placeholder="e.g., +123456789">
          </div>
          <!-- Address -->
          <div class="form-group">
            <label for="address" class="text-black">Address:</label>
            <textarea v-model="formData.address" class="form-control" id="address" name="address" rows="3"></textarea>
          </div>
          <!-- Add other fields as needed -->
          <div class="d-flex justify-content-between mt-2">
            <button type="submit" class="btn btn-primary">Update Details</button>
            <button type="button" class="btn btn-danger" @click="deleteAccount">Delete Account</button>
          </div>
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import store from '@/store/index'; 
  
  export default {
    setup() {
      const formData = ref({
        firstName: '',
        lastName: '',
        userAge: 18,
        gender: 'Male',
        userProfile: '',
        phoneNumber: '',
        address: '',
      });
  
      const imageUrl = ref('http://i.pravatar.cc/500?img=7');
  
      const updateUser = async () => {
        try {
          const userId = store.state.auth.userId;
          const payload = {
            id: userId,
          };
  
          const { msg } = await store.dispatch('updateUser', payload);
  
          if (msg) {
            console.log('Update successful:', msg);
          }
        } catch (error) {
          console.error('Error updating user:', error);
        }
      };
  
      const handleImageUpload = async (event) => {
        const input = event.target;
        if (input.files && input.files[0]) {
          const reader = new FileReader();
          reader.onload = async () => {
            const image = new Image();
            image.src = reader.result;
            image.onload = async () => {
              const canvas = document.createElement('canvas');
              const maxSize = 1024; // Define the maximum size in pixels
              let width = image.width;
              let height = image.height;
  
              // Resize the image if it exceeds the maximum dimensions
              if (width > maxSize || height > maxSize) {
                if (width > height) {
                  height *= maxSize / width;
                  width = maxSize;
                } else {
                  width *= maxSize / height;
                  height = maxSize;
                }
              }
  
              canvas.width = width;
              canvas.height = height;
              const ctx = canvas.getContext('2d');
              ctx.drawImage(image, 0, 0, width, height);
  
              // Convert the canvas content to data URL
              const resizedDataURL = canvas.toDataURL('image/jpeg');
  
              // Set the resized image URL to imageUrl
              imageUrl.value = resizedDataURL;
  
              // Set the resized image data URL to formData.userProfile
              formData.value.userProfile = resizedDataURL;
  
              // Dispatch action to update user image URL in the store
              store.dispatch('updateUserImage', resizedDataURL); // Or pass the resizedDataURL to the updateUserImage action
  
              console.log('Image uploaded successfully.');
              console.log(resizedDataURL);
            };
          };
  
          reader.readAsDataURL(input.files[0]);
        }
      };
  
      return {
        formData,
        updateUser,
        handleImageUpload,
        imageUrl,
      };
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 800px;
    margin: 0 auto;
    color: white;
    padding: 20px;
  }
  
  .form-group label {
    font-weight: bold;
  }
  
  .btn-primary {
    background-color: #7c3a7a;
    border-color: #7c3a7a;
    transition: background-color 0.3s; 
  }
  
  .btn-danger {
    background-color: #750000;
    border-color: #750000;
    transition: background-color 0.3s; 
  }
  
  .btn-primary:hover,
  .btn-danger:hover {
    background-color: #ae5d9d; 
  }
    
  .cursive {
    font-family: cursive;
    font-size: 2.5rem;
  }
  </style>
  