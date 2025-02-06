<template>
  <div class="comments-container">
    <h1 class="comments-title">Comments</h1>
    <ul class="comments-list">
      <li v-for="comment in comments" :key="comment.id" class="comment-item">
        <strong>{{ comment.name }}</strong>
        <p>{{ comment.comment }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '../lib/supabaseClient'

const comments = ref([])

async function getComments() {
  const { data } = await supabase.from('comments').select()
  comments.value = data
}

onMounted(() => {
  getComments()
})
</script>

<style scoped>
/* Container for the comments section */
.comments-container {
  max-width: 800px;
  margin: 40px auto;
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* Title for the comments section */
.comments-title {
  text-align: center;
  font-size: 32px;
  color: #2d3e50; /* Dark Slate Blue */
  margin-bottom: 20px;
  font-weight: 600;
}

/* List styling */
.comments-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

/* Individual comment item styling */
.comment-item {
  padding: 12px;
  margin-bottom: 15px;
  background-color: #fafafa; /* Light background for each comment */
  border-radius: 8px;
  border-left: 4px solid #007bff; /* Accent line for each comment */
  transition: background-color 0.3s ease;
}

.comment-item:hover {
  background-color: #f0f8ff; /* Light Blue Hover effect */
}

/* Name styling */
.comment-item strong {
  font-size: 18px;
  color: #333;
  font-weight: 600;
}

/* Comment text styling */
.comment-item p {
  font-size: 16px;
  color: #555;
  margin-top: 5px;
}
</style>