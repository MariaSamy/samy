Can you correct my code

const apiUrl = 'http://localhost:3000/get-response';

const chatMessagesDiv = document.getElementById('chat-messages');
const userInput = document.getElementById('user-input');
const sendButton = document.getElementById('send-button');

// Function to display chat messages
function displayMessage(message, sender) {
  const messageDiv = document.createElement('div');
  messageDiv.classList.add('chat-message');
  messageDiv.textContent = `${sender}: ${message}`;
  chatMessagesDiv.appendChild(messageDiv);
}

// Function to send user message to backend and receive AI response
async function getAIResponse(userMessage) {
  try {
    const response = await fetch(apiUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ message: userMessage }),
    });

    const data = await response.json();
    return data.response; // The AI response is extracted from the JSON data
  } catch (error) {
    console.error('Error calling the backend API:', error);
    return 'Failed to get AI response';
  }
}

// Function to handle user input and AI response
async function handleMessage() {
  const userMessage = userInput.value.trim();

  if (userMessage !== '') {
    displayMessage(userMessage, 'You');

    const aiResponse = await getAIResponse(userMessage);

    if (aiResponse !== 'Failed to get AI response') {
      // Display the AI response if it's not an error message
      displayMessage(aiResponse, 'AI');
    } else {
      // Handle the error case and display the actual error message received from the backend
      displayMessage('Oops, something went wrong: ' + aiResponse, 'AI');
    }

    userInput.value = ''; // Clear the user input field
  }
}

// Event listener for the send button
sendButton.addEventListener('click', handleMessage);

// Event listener for pressing Enter key in the input field
userInput.addEventListener('keyup', (event) => {
  if (event.key === 'Enter') {
    handleMessage();
  }
});
