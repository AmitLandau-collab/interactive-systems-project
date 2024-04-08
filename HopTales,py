from IPython.core.async_helpers import _trio_runner
from numpy import random
import streamlit as st
import pandas as pd
from streamlit.elements.widgets.number_input import Number
import google.generativeai as genai
import time
from streamlit_option_menu import option_menu
import re
import openai
import streamlit_star_rating
from streamlit_star_rating import st_star_rating
openai.api_key = ''
genai.configure(api_key="")

#### users control
if 'user_data' not in st.session_state:
    st.session_state['user_data'] = pd.DataFrame({
    'Name': ['amit', 'katya'],
    'Age': [21, 24],
    'Gender': ['Boy', 'Girl']
})

if "curr_user" not in st.session_state: 
  st.session_state['curr_user']={}

### story creation and presentation
# st.session_state['settings']={"moral":"white men are evil","rhymes":"yes","setting":"snowy mountains","character":"pier the bear"}
if 'settings' not in st.session_state:
  st.session_state['settings'] = {"moral":"Lunch is important","rhymes":"yes","setting":"kitchen","character":"pier the bear"}
if 'prompt_in' not in st.session_state:
    st.session_state['prompt_in'] = False
if 'photos' not in st.session_state:
    st.session_state['photos'] = []

if 'curr_story_A' not in st.session_state:
  st.session_state['curr_story_A'] = ["a", "b", "c","d","e","f","g"]

if 'decision_A' not in st.session_state:
  st.session_state['decision_A'] = "confront"

if 'curr_story_B' not in st.session_state:
  st.session_state['curr_story_B'] = ["uno", "dos", "tres","cuatro","cinco","seis","siete"]

if 'decision_B' not in st.session_state:
  st.session_state['decision_B'] = "hide"

if 'current_story' not in st.session_state:
  st.session_state['current_story'] = "A"

if "curr_story_title" not in st.session_state:
  st.session_state['curr_story_title'] = "amit's story"

if "current_index" not in st.session_state:
    st.session_state["current_index"] = 0

if "story_saved" not in st.session_state:
    st.session_state["story_saved"] = False

if "stars" not in st.session_state:
  st.session_state['stars']= 3
## story collection
if 'created_stories' not in st.session_state:
  st.session_state['created_stories']= pd.DataFrame(columns=['user', 'title','story_A',"story_B","photos","stars"])
random_characters = ["Danny", "Sarah", "Lily", "Michael", "Oliver", "Emma", "Max", "Ella", "Lucy", "Leo", "Sophie", "Charlie", "Mia", "Jack", "Grace", "Ruby", "Luke", "Luna the Unicorn", "Piper the Bear", "Rosie the Rabbit", "Milo the Fox", "Oscar the Owl", "Sammy the Squirrel", "Toby the Turtle", "Max the Mouse", "Charlie the Chipmunk", "Lola the Lion", "Mia the Monkey", "Felix the Frog", "Leo the Lion", "Benny the Bunny", "Cooper the Cat", "Molly the Mouse", "Ella the Elephant"]
random_morals = ["Be kind to others", "Share with friends", "Always tell the truth", "Use kind words", "Help those in need", "Respect nature", "Believe in yourself", "Never give up", "Be grateful for what you have", "Be honest and trustworthy", "Take turns and share toys", "Listen to others", "Practice good manners", "Take care of animals", "Be brave and face your fears", "Use your imagination", "Learn from your mistakes", "Follow your dreams", "Stay positive", "Apologize when you're wrong", "Take responsibility for your actions", "Be patient and wait your turn", "Value friendship", "Be curious and ask questions", "Be mindful of others' feelings", "Appreciate diversity", "Be respectful to elders", "Stand up for what is right", "Keep trying and never stop learning", "Treat others the way you want to be treated", "Think before you act", "Look for the good in every situation", "Be compassionate and empathetic", "Take care of the environment", "Be honest about your feelings", "Forgive others", "Set goals and work hard to achieve them", "Learn to share and cooperate", "Be mindful of your actions", "Value honesty and integrity", "Stay humble and appreciate others"]
random_locations = ["Quiet forest", "Cozy home", "Sunny meadow", "Busy market", "Peaceful beach", "Colorful playground", "Animal farm", "Sparkling river", "Green park", "Friendly neighborhood", "Magical garden", "Misty lake", "Snowy hill", "Starlit campsite", "Charming bakery", "Bustling street", "Rocky trail", "Old treehouse", "Friendly classroom", "Enchanted pond", "Hidden cave", "Rainy day in city", "Warm kitchen", "Sailing on a boat", "At kindergarten yard", "Merry carnival", "Peaceful backyard", "Cosy den", "Sunny rooftop", "Magical forest clearing", "Little village", "Busy workshop", "His birthday party", "Secret clubhouse", "Wildflower field", "On the riverbank", "Grandparent's house", "Toy-filled playroom"]
#### page functions and helper functions

## landing page 
def home_page():
    # Apply global styles including the new styles for the title and buttons
    st.markdown("""
        <style>
        .stApp {
            background-image: url('https://live.staticflickr.com/65535/53635656839_8b93ba138a_m.jpg');
            background-repeat: no-repeat;
            background-attachment: fixed;
            background-size: cover;
        }
        .stButton>button {
            font-size: 20px; /* Larger font size for buttons */
            border-radius: 30px; /* Rounded edges for buttons */
            border: none; /* No border by default */
            padding: 10px 24px; /* Padding inside the buttons */
            margin: 0px 10px; /* Margin between buttons */
        }
        .stButton>button:hover {
            background-color: #F4A261; /* Sunset Orange for button hover */
            transform: scale(1.1); /* Slightly increase button size on hover */
            transition-duration: 0.3s; /* Smooth transition for hover effect */
            color: white; /* Change text color to white on hover */
        }
        h1 {
            text-align: center;
            font-family: 'Comic Sans MS', 'Comic Neue', cursive;
            font-size: 4em;
            color: #336699; /* Soft blue for the title */
            margin-bottom: 0px; /* Remove bottom margin */
        }
        h2 {
            text-align: center;
            font-family: 'Comic Sans MS', 'Comic Neue', cursive;
            font-size: 2.5em;
            color: #33aabb; /* A matching lighter shade for the subtitle */
            margin-top: 0px; /* Remove top margin for spacing */
        }
        .stMarkdown {
            text-align: center;
            font-family: 'Comic Sans MS', 'Comic Neue', cursive;
            color: #445566;
            font-size: 1.5em;
        }
        </style>
        """, unsafe_allow_html=True)

    # Center-align the title and subtitle with updated text styles
    st.markdown("<h1>HopTales</h1>", unsafe_allow_html=True)
    st.markdown("<h2>Jump into Stories</h2>", unsafe_allow_html=True)

    # Add an engaging and colorful introduction
    st.markdown("""
        <div class="stMarkdown">
            Welcome to HopTales, where stories come to life! Dive into a world of imagination and creativity, 
            where every tale is an adventure waiting to be explored. Create your own stories, explore the magical 
            realms of others, and let your imagination soar. Start your storytelling journey today!
        </div>
        """, unsafe_allow_html=True)
    
    # Create columns for Sign In and Sign Up buttons with proper spacing and alignment
    col1, col2, col3, col4 = st.columns([1,1,1,1], gap="medium")
    with col2:
        if st.button('Sign In'):
            st.session_state['current_page'] = 'sign_in'
            st.rerun()

    with col3:
        if st.button('Sign Up'):
            st.session_state['current_page'] = 'sign_up' 
            st.rerun()

       
def add_user(user_data, name, age, gender):
    new_user = pd.DataFrame({'Name': [name], 'Age': [age], 'Gender': [gender]})
    user_data = pd.concat([user_data, new_user], ignore_index=True)
    st.session_state['user_data'] = user_data

def user_icon_button(name, gender, key):
    if gender == 'boy':
        icon_url = 'https://live.staticflickr.com/65535/53635496636_2dca69c82b_m.png'  # Replace with your actual boy icon path
    elif gender == 'girl':
        icon_url = 'https://www.flaticon.com/free-animated-icon/girl_15366223.gif'  # Replace with your actual girl icon path
    else:
        icon_url = 'path_to_default_icon.png'

def sign_up_page():
    st.title('Sign Up')
    st.write('Please fill out the form below to sign up.')

    # Input fields for name, age, and gender
    name = st.text_input('Write your name')
    age = st.text_input('Write your age')
    gender = st.radio("Gender", ["boy", "girl"])  # Gender radio button

    st.session_state["finish_profile"] = False

    if st.button('Finish Profile'):
        # Validation checks
        if not name:
            st.warning('Please enter your name.')
        elif name in st.session_state['user_data']['Name'].values:
            st.warning('Name already exists.')
        elif not age:
            st.warning('Please enter your age.')
        elif not gender:  # Ensure gender is selected
            st.warning('Please select your gender.')
        else:
            st.success('Profile completed successfully!')
            add_user(st.session_state['user_data'], name, age, gender)  # Pass gender here
            st.session_state["finish_profile"] = True
            st.session_state['current_page'] = 'home'

    if st.session_state["finish_profile"]:
        if st.button("Go to Home Page"):
            home_page()


def sign_in_page():
    st.markdown("""
    <style>
    .stApp {
        background-image: url('https://live.staticflickr.com/65535/53635672698_e40d9c5d24_m.jpg');
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center;
    }
    .stTextInput>div>div>input {
        border-radius: 20px;
        border: 1px solid #aaa; /* Light grey border */
        padding: 10px;
        font-size: 18px; /* Larger font for readability */
    }
    .stButton>button {
        border-radius: 30px; /* Rounded edges for buttons */
        padding: 15px 30px;
        font-size: 18px;
        background-color: #F4A261; /* Playful button color */
        border: none;
        color: white;
        transition: transform 0.3s;
    }
    .stButton>button:hover {
        transform: scale(1.1);
    }
    h2 {
        text-align: center;
        font-family: 'Comic Sans MS', 'Comic Neue', cursive;
        color: #336699; /* Choose a color that fits the theme */
        margin-bottom: 40px;
    }
    /* Custom styles for user buttons and images */
    .user-btn {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
    .user-btn .stButton>button {
        margin-top: 10px; /* Space between image and button */
    }
    .stImage {
        display: flex;
        justify-content: center;
    }
    .stImage>img {
        border-radius: 50%; /* Circular images */
        width: 1000px; /* Width of image */
        height: 1000px; /* Height of image */
    }
    </style>
    """, unsafe_allow_html=True)
    cols = st.columns([1] * (len(st.session_state['user_data']) + 1), gap="small")
    for i, user in st.session_state['user_data'].iterrows():
        with cols[i]:
            st.markdown(f"<div class='user-btn'>", unsafe_allow_html=True)
            gender_icon = 'https://live.staticflickr.com/65535/53635496636_2dca69c82b_m.jpg' if user['Gender'].lower() == 'boy' else 'https://live.staticflickr.com/65535/53635711188_721fe2cc86_m.jpg'
            st.image(gender_icon, width=150, output_format='PNG')
            if st.button(f"Sign In as {user['Name']}"):
                sign_in(user.to_dict(), user['Gender'])
                st.rerun()
            st.markdown("</div>", unsafe_allow_html=True)
    
    # Button for creating a new user
    with cols[-1]:
        st.markdown(f"<div class='user-btn'>", unsafe_allow_html=True)
        if st.button("Create New User"):
            st.session_state['current_page'] = 'sign_up'
            st.rerun()

            sign_up_page()
        st.markdown("</div>", unsafe_allow_html=True)


def sign_in(user_dict, gender):
    st.session_state['curr_user'] = user_dict
    st.write(f"User {user_dict['Name']} is signed in.")
    if gender.lower() == 'boy':
        st.image('https://live.staticflickr.com/65535/53635496636_2dca69c82b_m.jpg', use_column_width=True)
    elif gender.lower() == 'girl':
        st.image('https://live.staticflickr.com/65535/53635711188_721fe2cc86_m.jpg', use_column_width=True)
    st.session_state['current_page'] = 'main_menu'
    

## main menu page
def main_menu_page():
    # Retrieve current user's information
    current_user = st.session_state.get('curr_user', {})
    user_name = current_user.get('Name', 'User')
    user_gender = current_user.get('Gender', '').lower()
    
    # Define user icon URL based on gender
    user_icon_url = 'https://live.staticflickr.com/65535/53635496636_2dca69c82b_m.jpg' if user_gender == 'boy' else 'https://live.staticflickr.com/65535/53635711188_721fe2cc86_m.jpg'
    # Custom styles
    st.markdown("""
    <style>
    /* Main background color */
    .stApp {
        background-color: #F0E5CF; /* Soft cream background */
    }

    /* Title style */
    h1 {
        color: #305F72; /* Deep sea blue for title */
        font-family: 'Comic Sans MS', 'Comic Neue', cursive;
        text-align: center;
        margin-bottom: 0.5em;
    }

    /* Welcome message style */
    .welcome-msg {
        color: #305F72; /* Matching the title color */
        font-size: 24px; /* Readable size for welcome text */
        font-family: 'Comic Sans MS', 'Comic Neue', cursive;
        display: flex;
        align-items: center;
        justify-content: left;
        padding-left: 20px; /* Padding from the left edge */
    }

    /* User icon style */
    .user-icon {
        width: 80px; /* Visible size for user icon */
        height: 80px;
        border-radius: 50%; /* Circular icon */
        margin-right: 15px; /* Space between icon and text */
    }

    /* Button styles */
    .stButton > button {
        color: #FFFFFF; /* White text color */
        background-color: #FFADAD; /* Soft red for buttons */
        padding: 10px 20px; /* Spacious padding for touch friendliness */
        border-radius: 20px; /* Fully rounded edges for buttons */
        border: none; /* Remove default border */
        font-family: 'Comic Sans MS', 'Comic Neue', cursive;
        font-size: 20px;
        margin: 10px 0; /* Margin between buttons */
        width: 90%; /* Button width relative to container */
        transition: background-color 0.3s; /* Smooth transition for hover effect */
    }

    .stButton > button:hover {
        background-color: #FFCACA; /* Lighter red on hover */
    }

    /* Button captions for extra information */
    .button-caption {
        color: #FFADAD; /* Color to match the button */
        font-size: 16px; /* Smaller font size for caption */
        margin-top: -5px; /* Negative margin to bring closer to button */
        text-align: center;
    }
    </style>
    """, unsafe_allow_html=True)

    # Welcome message with user icon
    st.markdown(f"""
    <div class="welcome-msg">
        <img src="{user_icon_url}" alt="User icon" class="user-icon">
        HELLO {user_name.upper()}
    </div>
    <h1>Select Story</h1>
    """, unsafe_allow_html=True)

    # Buttons with captions
    if st.button("Create New Story", key="create_new_story"):
        # action for button
        st.session_state['current_page'] = 'story_settings'
        st.experimental_rerun()
    st.markdown("<div class='button-caption'>Create a new story tailored for you!</div>", unsafe_allow_html=True)

    if st.button("Saved Stories", key="saved_stories"):
        # action for button
        st.session_state['current_page'] = 'saved_stories'
        st.experimental_rerun()
    st.markdown("<div class='button-caption'>Check your favorite stories</div>", unsafe_allow_html=True)

    if st.button("Change User", key="change user"):
        # action for button
        st.session_state['current_page'] = 'sign_in' # Update as necessary
        st.rerun()
    st.markdown("<div class='button-caption'>click to change profile</div>", unsafe_allow_html=True)

def story_settings_page():
  st.markdown("""
    <style>
        .stApp {
            background-color: #F5F5DC; /* Beige background */
        }
        .title {
            font-family: 'Comic Sans MS', 'Comic Neue', cursive;
            color: #38A647; /* Green color */
            font-size: 30px; /* Larger font size */
            margin-bottom: 20px; /* Space below the title */
        }
        .stTextInput>div>div>input, .stRadio>div>div>label>input {
            border-radius: 20px; /* Rounded edges for text input and radio buttons */
            border: 2px solid #38A647; /* Green border */
            padding: 5px 10px; /* Padding inside the inputs */
        }
        .stButton>button {
            font-size: 18px; /* Button font size */
            border: none;
            border-radius: 30px;
            padding: 10px 20px;
            margin: 10px 0; /* Space around buttons */
            color: white;
            background-color: #38A647; /* Green color */
        }
        .stButton>button:hover {
            background-color: #2e8b57; /* Darker green on hover */
        }
    </style>
    """, unsafe_allow_html=True)
  st.subheader("choose your story settings")
  setting = st.text_input("write the setting of the story")
  rhymes = st.radio("rhymes", ["no", "yes"])
  character = st.text_input("a character you want in the story")
  moral = st.text_input("write the moral of the story")
  st.session_state['story_saved']= False
  col1, col2 = st.columns(2)
  with col1:
      if st.button("create new story"):
        st.session_state["current_index"] = 0
        st.session_state['random_story'] = False
        st.session_state['settings']["setting"] = setting
        st.session_state['settings']["rhymes"] = rhymes
        st.session_state['settings']["moral"] = moral
        st.session_state['settings']["character"] = character
        {"moral":moral,"rhymes":rhymes,"setting":setting,"character":character}
        st.session_state['current_page'] = 'story_process'
        st.rerun()
  with col2:
      if st.button("create a random story"):
        st.session_state["current_index"] = 0
        st.session_state['random_story'] = True
        st.session_state['current_page'] = 'story_process'
        st.rerun()
  if st.button("Main Menu"):
    st.session_state['current_page'] = 'main_menu'
    st.rerun()

## story generation   
def create_prompt():
  model = genai.GenerativeModel('gemini-pro')
  genai.configure(api_key="AIzaSyAAwDwJMXWOKlvo7GWHVXsYJ0LUDsQalJM")
  
  if not st.session_state['settings']["character"] or st.session_state['random_story'] == True:
    st.session_state['settings']["character"] = random.choice(random_characters)
    character = st.session_state['settings']["character"]
  else: character = st.session_state['settings']["character"]

  if not st.session_state['settings']["moral"] or st.session_state['random_story'] == True:
    st.session_state['settings']["moral"] = random.choice(random_morals)
    moral =st.session_state['settings']["moral"]
  else: moral = st.session_state['settings']["moral"]
  
  if not st.session_state['settings']["setting"] or st.session_state['random_story'] == True:
    st.session_state['settings']["setting"] = random.choice(random_locations) 
    setting =st.session_state['settings']["setting"]
  setting = st.session_state['settings']["setting"]
  rhymes = st.session_state['settings']["rhymes"]
  ### use gemini to create a random story
  prompt = (
  f"Create a children's story with the following structure: start printing from the title and keep the section numbers\n\n"
  f"Moral: {moral}\n"
  f"Character: {character}\n"
  f"Title: make a title based on the setting, special event, not the dillema, and character. up to 5 words\n"
  f"1. Introduce {character} and the{setting} setting.\n"
  f"2. Image Prompt:write a Describtion to draw a cartoon image of {character} cartoon style in the setting, mention their appearance and emotion no need to rhym write it as a prompt not a part of the story .\n"
  f"3. {character} does a normal thing that shows his character.\n"
  f"4. the normal things leads to a special event. the event presents {character} with a dillema, relevant to the moral of the story. "
  f"5. Decision Question: Ask the reader to choose between two options for {character}.\n\n"
  f"Option A: name of the option\n"
  f"6. Describe what {character} does based on the first choice and the outcome.\n"
  f"7. describe how the action {character} took and the result changed {character}'s character "
  f"8. conclude the events of the story, while showing the character growth of {character}."
  f"9. A paragraph that emphasizes the moral of the story as demonstrated by {character}'s actions in Option A.\n"
  f"10. Image Prompt: Conclude the story with an image that reflects the moral and shows {character} in cartoon style in the final setting.no need to rhym write it as a prompt not a part of the story\n\n"
  f"Option B: name of the option\n"
  f"11. Describe what {character} does based on the first choice and the outcome.\n"
  f"12. describe how the action {character} took and the result changed {character}'s character "
  f"13. conclude the events of the story, while showing the character growth of {character}."
  f"14. A paragraph that emphasizes the moral of the story as demonstrated by {character}'s actions in Option A.\n"
  f"15. Image Prompt: Conclude the story with an image that reflects the moral and shows {character} in cartoon style in the final setting.no need to rhym write it as a prompt not a part of the story\n\n"
  )

  if rhymes == "yes":
      prompt += "\nThe story should rhyme. don't rhyme the image prompts"
  prompt += "\nEnsure the content is appropriate for children, every paragraph is 4 rows."

  if st.session_state['settings']['rhymes'] == "yes":
    prompt += "Ensure the story is written in a rhyming structure.\n\n"
  prompt += "keep the story telling suitable for children and "
  response = model.generate_content(
  prompt,
  generation_config=genai.types.GenerationConfig(
      # Only one candidate for now.
      candidate_count=1,
      temperature=0.7
    )
  )
  try:
    return response.parts[0].text
  except ValueError:
    st.subheader(" violent languauge detected, please change settings")  
    if st.button('back to settings'):
      st.session_state['current_page'] = 'story_settings'
      st.rerun()

def extract_image_prompts(story_text):
    image_prompts = []

    # Split the story into lines
    lines = story_text.split('\n')

    # Iterate over each line
    for i in range(len(lines)):
        # Check if the line contains "Image Prompt:"
        if "Image Prompt:" in lines[i]:
            # Extract the prompt by joining lines until an empty line is encountered
            prompt = lines[i].split("Image Prompt:")[-1].strip()
            image_prompts.append(prompt)

    return image_prompts

def story_process_page():
  st.session_state['story_prompt'] = create_prompt()
  if st.session_state['story_prompt'] is not None:
      
      st.session_state['curr_story_title']= st.session_state['story_prompt'].split('\n\n')[0].replace('*', '')
      paragraphs = st.session_state['story_prompt'].split('\n\n')[1:]
      paragraphs = [para[2:] for para in paragraphs]
      st.session_state["curr_story_A"]= paragraphs[:1] + paragraphs[2:5] + paragraphs[6:10]
      st.session_state["curr_story_B"]= paragraphs[:1] + paragraphs[2:5]+paragraphs[12:]
      st.session_state["decision_A"] = paragraphs[5].split(':')[1].replace('*', '')
      try:
        st.session_state["decision_B"] = paragraphs[11].split(':')[1].replace('*', '')
      except:
        st.rerun()
      st.session_state['photos']=[]
      #1,6,11 

      image_paragraphs = [paragraphs[0], paragraphs[5], paragraphs[10]]

      for prompt in image_paragraphs:
            # Generate the image using DALL·E
            response = openai.Image.create(
                prompt=prompt,
                n=1,
                size="256x256"
            )
            # Extract the URL of the generated image
            image_url = response['data'][0]['url']
            st.session_state['photos'].append(image_url)
      print(f"path A is:{st.session_state['curr_story_A']}")
      print(f"path B is:{st.session_state['curr_story_B']}")
      print(f' decision A: {st.session_state["decision_A"]}')
      st.session_state['current_page'] = 'story_show'
      st.rerun()
        #st.session_state['prompt_in']=False

## story presentation


def story_move():
    if st.session_state["story_move_page"]: 
        st.session_state["current_index"] += 1
    else:
        st.session_state["current_index"] -= 1

def story_show_page():
    # Define CSS styles for the page
    st.markdown("""
        <style>
            .stApp {
                background-color: #F5F5DC; /* Beige background */
            }
            h1.title {
                font-family: 'Comic Sans MS', 'Comic Neue', cursive;
                color: #38A647; /* Adjust for your preferred shade of green */
                font-size: 40px; /* Increased font size */
                text-align: center;
                margin: 20px 0; /* Add some space around the title */
            }
            .text-and-image {
                display: flex;
                align-items: center;
                justify-content: start;
            }
            .text-and-image .stImage {
                margin-right: 10px; /* Add some space between the image and the text */
            }
            .stButton>button.next-button {
                color: white;
                background-color: #38A647; /* Green color for the 'Next' button */
            }
            .stButton>button.back-button {
                color: white;
                background-color: #A9A9A9; /* Grey color for the 'Back' button */
            }
        </style>
        """, unsafe_allow_html=True)

    # Title of the story
    st.markdown(f"<h1 class='title'>{st.session_state['curr_story_title']}</h1>", unsafe_allow_html=True)
   
            
    if "story_move_page" not in st.session_state:
        st.session_state["story_move_page"] = True

    question_placeholder = st.empty()

    if st.session_state["current_index"] == len(st.session_state['curr_story_A'])-1: 
        paragraph = st.session_state[f'curr_story_{st.session_state["current_story"]}'][st.session_state["current_index"]]
        with question_placeholder.container():
            if st.session_state["current_story"]== 'A':
              st.image(st.session_state['photos'][1])
            else: st.image(st.session_state['photos'][2])
            st.text_area(st.session_state['curr_story_title'], value=paragraph, height=150)
            col1, col2 = st.columns(2)
            with col1:
                if st.button("Last Page"):
                  st.session_state["story_move_page"] = False
                  story_move()
                  st.rerun()
            with col2:
                if st.button("Finish_Story"):

                  st.session_state["story_move_page"] = True
                  story_move()
                  st.rerun()

    elif st.session_state["current_index"] == 0: 
      paragraph = st.session_state[f'curr_story_{st.session_state["current_story"]}'][st.session_state["current_index"]]
      with question_placeholder.container():
        st.image(st.session_state['photos'][0])
        st.text_area(st.session_state['curr_story_title'],value = paragraph,height = 150)
        col1, col2 = st.columns(2)
        with col1:
          if st.button("settings"):
            st.session_state['current_page'] = 'story_settings'
            st.rerun()
        with col2:
          if st.button("next page"):
              st.session_state["story_move_page"] = True
              story_move()
              st.rerun()

    elif st.session_state["current_index"] == 3: 
      paragraph = st.session_state[f'curr_story_{st.session_state["current_story"]}'][st.session_state["current_index"]]
      with question_placeholder.container():
        st.text_area(st.session_state['curr_story_title'],value = paragraph,height = 150)
        col1, col2 = st.columns(2)
        with col1:
          if st.button(st.session_state["decision_A"]):
            st.session_state["story_move_page"] = True
            st.session_state["current_story"]= "A"
            story_move()
            st.rerun()
        with col2:
          if st.button(st.session_state['decision_B']):
              st.session_state["story_move_page"] = True
              st.session_state["current_story"]= "B"
              story_move()
              st.rerun()
        if st.button("last page"):
              st.session_state["story_move_page"] = False
              story_move()
              st.rerun()      

    elif st.session_state["current_index"] != len(st.session_state['curr_story_A']): 
      paragraph = st.session_state[f'curr_story_{st.session_state["current_story"]}'][st.session_state["current_index"]]
      with question_placeholder.container():
          st.text_area(st.session_state['curr_story_title'],value = paragraph,height = 150)
          col1, col2 = st.columns(2)
          with col1:
            if st.button("last page"):
              st.session_state["story_move_page"] = False
              story_move()
              st.rerun()
          with col2:
            if st.button("next page"):
              st.session_state["story_move_page"] = True
              story_move()
              st.rerun()
    else:        
      st.session_state['current_page'] = 'story_feedback'
      st.rerun()

## story feedback page
def change_save():
  if (st.session_state['curr_story_title'] in st.session_state['created_stories']['title'].values):
    print("deleting story")
    st.session_state["created_stories"] = st.session_state["created_stories"][st.session_state["created_stories"]["title"] != st.session_state["curr_story_title"]]
  else:
    print("adding story")
    story_to_add =pd.DataFrame({'user': st.session_state['curr_user']['Name'], 'title': st.session_state['curr_story_title'], 'story_A': [st.session_state['curr_story_A']], 'story_B': [st.session_state['curr_story_B']], "photos":[st.session_state['photos']],"stars": st.session_state['stars']})
    st.session_state['created_stories'] = pd.concat([st.session_state['created_stories'],story_to_add], ignore_index=True)
from streamlit_star_rating import st_star_rating

    
def saved_stories_page():
  st.markdown("""
    <style>
        .stApp {
            background-color: #F5F5DC; /* Beige background */
        }
        .title {
            font-family: 'Comic Sans MS', 'Comic Neue', cursive;
            color: #38A647; /* Green color */
            font-size: 30px; /* Larger font size */
            margin-bottom: 20px; /* Space below the title */
        }
        .story-button {
            display: block;
            width: 100%;
            margin: 10px 0;
            padding: 10px;
            border-radius: 20px;
            border: 2px solid #38A647;
            text-align: left;
            color: #38A647;
            font-size: 18px;
        }
        .story-button:hover {
            background-color: #38A647;
            color: white;
        }
    </style>
    """, unsafe_allow_html=True)
  st.markdown("<h1 class='title'>Saved Stories</h1>", unsafe_allow_html=True)
  st.session_state['story_saved']= True
  print(st.session_state['created_stories'])
  user_stories= st.session_state['created_stories'][st.session_state["created_stories"]["user"] == st.session_state["curr_user"]["Name"]].sort_values(by='stars', ascending=False)
  col1, col2 = st.columns(2)
  for i,user in user_stories.iterrows():
    with col1:
      if st.button(user['title']):
        st.session_state['curr_story_A'] = user['story_A']
        st.session_state['curr_story_B'] = user['story_B']
        st.session_state['curr_story_title'] = user['title']
        st.session_state['photos']= user['photos']
        st.session_state['current_index']= 0
        st.session_state['current_page'] = 'story_show'
        st.rerun()
    with col2:
      star ='\u2605'
      st.write(user['stars']*star)      
  if st.button("Go Back to Main Menu"):
    st.session_state['current_page'] = 'main_menu'
    st.rerun()



def story_feedback_page():
  st.markdown("""
    <style>
        .stApp {
            background-color: #F5F5DC; /* Beige background */
        }
        .title {
            font-family: 'Comic Sans MS', 'Comic Neue', cursive;
            color: #38A647; /* Green color */
            font-size: 30px; /* Larger font size */
            margin-bottom: 30px; /* Space below the title */
        }
        .stButton>button {
            font-size: 18px; /* Button font size */
            border: none;
            border-radius: 30px;
            padding: 10px 20px;
            margin: 10px 0; /* Space around buttons */
        }
        .save-button {
            color: white;
            background-color: #38A647; /* Green color */
        }
        .save-button:hover {
            background-color: #2e8b57; /* Darker green on hover */
        }
    </style>
    """, unsafe_allow_html=True)

    # Page title

    # Toggle save story
  st.subheader(st.session_state['curr_story_A'][0])  
  st.write(st.session_state['story_saved'])
  on = st.toggle("click to save the story",value =st.session_state['story_saved'])

  if on:
    if st.session_state['story_saved']:
      st.write("story saved!")
    else: 
      change_save() 
      st.session_state['story_saved']= True
      st.rerun() 
  else:
    if st.session_state['story_saved']:
      change_save()
      st.session_state['story_saved']= False
      st.rerun() 
  
# Feedback form with star rating
# Feedback form with star rating
    
  st.session_state['stars'] = st_star_rating(label="Please rate your experience", maxValue=5, defaultValue=st.session_state['stars'], key="rating")

    # Submit button for the feedback
  if st.button("Submit Feedback", key="submit_feedback"):
    st.balloons()
  col1, col2 = st.columns(2)
  with col1:
      if st.button('read story again'):
        st.session_state["current_index"]= 0
        st.session_state['current_page'] = 'story_show'
        st.rerun()
            
  with col2:
      if st.button('main_menu'):
        st.session_state['current_page'] = 'main_menu'
        st.rerun()

def extract_image_prompts(story_text):
    image_prompts = []

    # Split the story into lines
    lines = story_text.split('\n')

    # Iterate over each line
    for i in range(len(lines)):
        # Check if the line contains "Image Prompt:"
        if "Image Prompt:" in lines[i]:
            # Extract the prompt by joining lines until an empty line is encountered
            prompt = lines[i].split("Image Prompt:")[-1].strip()
            image_prompts.append(prompt)

    return image_prompts

### main app
  
# Depending on the current page, render the appropriate screen

if 'current_page' not in st.session_state:
  st.session_state['current_page'] = 'home'
  home_page()

elif st.session_state['current_page'] == 'home':
    home_page() 
elif st.session_state['current_page'] == 'sign_in':
    sign_in_page()                
elif st.session_state['current_page'] == 'sign_up':
    st.write('Sign Up page')
    sign_up_page()
elif st.session_state['current_page'] == 'main_menu':
    main_menu_page()
elif st.session_state['current_page'] == 'story_settings':
    story_settings_page()       
elif st.session_state['current_page'] == 'story_process':
  story_process_page()
elif st.session_state['current_page'] == 'story_show':
  story_show_page() 
elif st.session_state['current_page'] == 'saved_stories':
  saved_stories_page()
elif st.session_state['current_page'] == 'story_feedback':
  story_feedback_page()
