<template>
  <div class="comment-form">
    <h2>Leave a Comment</h2>
    <form @submit.prevent="submitComment">
      <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="name" required class="form-control" />
      </div>
      <div class="form-group">
        <label for="comment">Comment:</label>
        <textarea id="comment" v-model="comment" required class="form-control"></textarea>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
      <div v-if="submissionStatus" class="status-message">
        {{ submissionStatus }}
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { supabase } from '../lib/supabaseClient'

const name = ref('');
const comment = ref('');
const submissionStatus = ref(null);

const tableName = 'comments'; // Name of your Supabase table

async function submitComment() {
  submissionStatus.value = "Submitting...";
  try {
    const { error } = await supabase
      .from(tableName)
      .insert([{ name: name.value, comment: comment.value }]);

    if (error) {
      console.error("Error inserting comment:", error);
      submissionStatus.value = "Error submitting comment. Please try again.";
    } else {
      submissionStatus.value = "Comment submitted successfully!";
      name.value = ''; // Clear form fields
      comment.value = '';
    }
  } catch (err) {
    console.error("An unexpected error occurred:", err);
    submissionStatus.value = "An unexpected error occurred. Please try again later.";
  }
}
</script>

<style scoped>
/* Container for the form */
.comment-form {
  max-width: 600px;
  margin: 30px auto;
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* Form title */
.comment-form h2 {
  text-align: center;
  color: #333;
  font-size: 28px;
  margin-bottom: 20px;
  font-weight: 600;
}

/* Group for each input field */
.form-group {
  margin-bottom: 1rem;
}

/* Labels for input fields */
label {
  display: block;
  font-weight: 600;
  color: #555;
  margin-bottom: 5px;
}

/* Styling for inputs and textareas */
.form-control {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 6px;
  background-color: #fafafa;
  transition: border-color 0.3s ease, background-color 0.3s ease;
}

/* Focused state of input fields */
.form-control:focus {
  border-color: #007bff;
  background-color: #e8f4ff;
  outline: none;
}

/* Submit button styling */
.btn {
  width: 100%;
  padding: 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

/* Button hover effect */
.btn:hover {
  background-color: #0056b3;
}

/* Status message styling */
.status-message {
  margin-top: 15px;
  text-align: center;
  font-size: 16px;
  font-weight: 600;
}

/* Success and error messages */
.status-message.success {
  color: #4caf50; /* Success Green */
}

.status-message.error {
  color: #f44336; /* Error Red */
}
</style>
