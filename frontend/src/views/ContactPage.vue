<template>
    <div class="row container-fluid">
        <div class="col">
          <div class="col container">
            <div class="row text-center">
                <h1 class="call">Contact Us</h1>
                <p class="text-black">khvlu</p>
            </div>
            
            <div class="row"> <img src="https://i.ibb.co/NS1K4Pt/Screenshot-2024-03-22-120618.png" class="container img-fluid w-50 h-50" alt="responsive image">
                <p class="text-black">khvlu</p>
                <p class="text-black">khvlu</p>
                
                <h2 class="call"><i class="bi bi-person-lines-fill"></i> 068 564 2357</h2>
                <h2 class="call"><i class="bi bi-envelope-at-fill"></i> Forbidden@bliss.com</h2>
                <h2 class="call"><i class="bi bi-instagram"></i>instagram.com</h2>
                <h2 class="call"><i class="bi bi-facebook"></i> facebook.com</h2>
            </div>
            <div class="row"><h1></h1></div>
          </div>
        </div>
        <div class="col">
            <div class="contact-page">
      <!-- Contact form -->
      <form @submit.prevent="submitForm" action="https://formspree.io/f/xpzgnpee" method="POST" class="contact-form">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="formData.name" name="name" required>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="formData.email" name="email" required>
        <label for="message">Message:</label>
        <textarea id="message" v-model="formData.message" name="message" required></textarea>
        <button type="submit">Submit</button>
        <!-- Error message -->
        <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
      </form>
    </div></div>
        <div class="col">
           
            <h1 class="not text-black">Mon To Fri</h1>
            
            <h1 class="not text-black">Mon To Fri</h1>
            <h1 class="not">Available Hours:</h1>
            <h1 class="not text-black">Mond To Friday</h1>
            
            <h1 class="not fs-2">Monday To Friday</h1>
            <h1 class="not fs-2">06:00 to 19:00</h1>
            <h1 class="not text-black">Mond To Friday</h1>
            <h1 class="not text-black">Mond To Friday</h1>

            <h1 class="not fs-2 text-white shadow">Special Consultation Available</h1>
        </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
    name: 'ContactPage',
    setup() {
      // Form data
      const formData = ref({
        name: '',
        email: '',
        message: ''
      });
  
      // Error message
      const errorMessage = ref('');
  
      // Method to submit form
      const submitForm = () => {
        // Validate form fields
        if (!formData.value.name || !formData.value.email || !formData.value.message) {
          errorMessage.value = 'Please fill out all fields.';
          return;
        }
  
        // Send form data to backend (Form Spree)
        fetch('https://formspree.io/f/xpzgnpee', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(formData.value)
        })
        .then(response => {
          if (!response.ok) {
            throw new Error('Failed to submit form.');
          }
          // Reset form data
          formData.value = {
            name: '',
            email: '',
            message: ''
          };
          // Show success message or redirect user
          alert('Form submitted successfully!');
        })
        .catch(error => {
          errorMessage.value = 'Failed to submit form. Please try again later.';
          console.error(error);
        });
      };
  
      return { formData, errorMessage, submitForm };
    }
  }
  </script>
  
  <style scoped>
  .contact-page {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 80vh; 
  }
  
  .contact-form {
    width: 100%;
    padding: 20px; 
    background-color: #000000; 
    border-radius: 10px;
  }
  
  .contact-form label {
    color: #ffd700; 
    margin-bottom: 8px;
  }
  
  .contact-form input,
  .contact-form textarea {
    width: calc(100% - 20px); 
    padding: 10px;
    margin-bottom: 15px;
    border: none;
    border-radius: 5px;
    background-color: #5c3317; 
    color: #ffffff;
  }
  
  .contact-form button {
    width: calc(100% - 20px);
    padding: 10px;
    border: none;
    border-radius: 5px;
    background-color: #8b0000; 
    color: #ffffff; 
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .contact-form button:hover {
    background-color: #dc143c; 
  }
  
  .error-message {
    color: #ff0000; 
    margin-top: 10px;
  }

  .call {
    display: flex;
    justify-content: center;
  }

  .not {
    display: flex;
    justify-content: center;
  }

  .bi {
    color:#ffd700
  }


  .row {
    display: flex;
    justify-content: center;
  }
  </style>
  