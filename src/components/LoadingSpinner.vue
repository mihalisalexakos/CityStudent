<template>
    <div class="spinner-box">
      <div class="spinner"></div>
    </div>
  </template>
  
  <style scoped>
  .spinner-box {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100px;
  }
  
  .spinner {
    width: 40px;
    height: 40px;
    border: 4px solid var(--white);
    border-top: 4px solid var(--black);
    border-radius: 50%;
    animation: spin 1s linear infinite;
  }
  
  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
  </style>