# sowmya.s
# Load pre-trained GPT-2 model and tokenizer from Hugging Face
import time

class StudyBot:
    def __init__(self):
        self.sessions = []
        self.total_study_time = 0

    def add_study_session(self):
        subject = input("Enter the subject you're studying: ")
        study_duration = int(input(f"Enter the study duration for {subject} in minutes: "))
        break_duration = int(input(f"Enter your break duration after studying {subject} (in minutes): "))
        
        # Store session details
        self.sessions.append({
            "subject": subject,
            "study_duration": study_duration,
            "break_duration": break_duration
        })
        print(f"Study session for {subject} added! You will study for {study_duration} minutes and take a {break_duration}-minute break.\n")

    def start_study_sessions(self):
        # Loop through all study sessions
        for session in self.sessions:
            subject = session["subject"]
            study_duration = session["study_duration"]
            break_duration = session["break_duration"]

            # Start the study session
            print(f"Starting your {study_duration}-minute study session for {subject}...\n")
            self.study_timer(study_duration)
            self.take_break(break_duration)
        
        # Show total study time
        print(f"\nYou completed {len(self.sessions)} study session(s) totaling {self.total_study_time} minutes of study time.")

    def study_timer(self, study_duration):
        # Timer for the study session
        for remaining in range(study_duration * 60, 0, -1):
            mins, secs = divmod(remaining, 60)
            print(f"Study Time Remaining: {mins:02d}:{secs:02d}", end="\r")
            time.sleep(1)
        
        # Notify when the session ends
        print("\nGreat job! You've completed your study session.")
        self.total_study_time += study_duration

    def take_break(self, break_duration):
        # Inform the user to take a break
        print(f"Time to take a {break_duration}-minute break. Relax and recharge!\n")
        time.sleep(break_duration * 60)  # Sleep for the break duration (in minutes)

# Create an instance of StudyBot
study_bot = StudyBot()

# Add multiple study sessions
while True:
    study_bot.add_study_session()
    more_sessions = input("Would you like to add another study session? (y/n): ").lower()
    if more_sessions != 'y':
        break

# Start all study sessions
study_bot.start_study_sessions()
model_name = "gpt2"
model = GPT2LMHeadModel.from_pretrained(model_name)
tokenizer = GPT2Tokenizer.from_pretrained(model_name)

# Function to generate bot responses based on user input
def chat():
    print("Hello! I'm a chatbot built with GPT-2. Type 'quit' to exit the conversation.")
    
    # Start conversation loop
    while True:
        user_input = input("You: ")
        
        # Exit the chat if the user types 'quit'
        if user_input.lower() == "quit":
            print("Goodbye!")
            break
        
        # Encode the user's input and generate a response
        input_ids = tokenizer.encode(user_input, return_tensors="pt")
        
        # Generate a response with a max length of 100 tokens and avoid repeating phrases
        output = model.generate(input_ids, max_length=100, num_return_sequences=1, no_repeat_ngram_size=2)
        
        # Decode the generated response to text
        bot_response = tokenizer.decode(output[0], skip_special_tokens=True)
        
        # Print the bot's response
        print("Bot: " + bot_response)

# Start the chatbot
chat()
