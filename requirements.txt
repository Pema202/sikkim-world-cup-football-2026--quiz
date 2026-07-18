import streamlit as st
import time

st.set_page_config(page_title="Quiz Time", page_icon="🎉", layout="centered")

# MEDIUM FONT + PINK THEME
st.markdown("""
<style>
    .stApp {background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 50%, #fecfef 100%);}
    
    /* MEDIUM TEXT */
    h1 {font-size: 38px !important; color: #d63384; text-align: center; font-weight: 700;}
    h2 {font-size: 30px !important; color: #d63384; text-align: center; font-weight: 700;}
    h3 {font-size: 26px !important; color: #ff1493; text-align: center; font-weight: 600;}
    
    /* MEDIUM BODY TEXT */
    p, div, label, .stRadio label {font-size: 20px !important; color: #333; font-weight: 500;}
    .stMarkdown {font-size: 20px !important;}
    
    /* MEDIUM BUTTONS */
    .stButton>button {
        background-color: #ff69b4; 
        color: white; 
        border-radius: 20px; 
        font-size: 20px !important;
        font-weight: 600;
        padding: 12px 25px;
        width: 100%;
    }
    .stButton>button:hover {background-color: #ff1493;}
    
    /* MEDIUM FEEDBACK */
    .stSuccess, .stError, .stInfo, .stWarning {
        font-size: 20px !important;
        font-weight: 600;
        text-align: center;
    }
</style>
""", unsafe_allow_html=True)

questions = [
    # SIKKIM - 5 QUESTIONS
    {"q": "What is the capital of Sikkim?", "options": ["Gangtok", "Namchi", "Pelling", "Mangan"], "answer": "Gangtok"},
    {"q": "Which is the highest peak in Sikkim?", "options": ["Kanchenjunga", "Mount Everest", "Nanda Devi", "Kamet"], "answer": "Kanchenjunga"},
    {"q": "Sikkim became the 22nd state of India in which year?", "options": ["1975", "1947", "1980", "1962"], "answer": "1975"},
    {"q": "Which famous monastery is in Sikkim?", "options": ["Rumtek Monastery", "Hemis Monastery", "Tawang Monastery", "Thiksey Monastery"], "answer": "Rumtek Monastery"},
    {"q": "What is the main language spoken in Sikkim?", "options": ["Nepali", "Bengali", "Assamese", "Hindi"], "answer": "Nepali"},
    
    # WORLD CUP FOOTBALL 2026 - 7 QUESTIONS
    {"q": "Which 3 countries will host the FIFA World Cup 2026?", "options": ["USA, Canada, Mexico", "USA, Brazil, Argentina", "Spain, Portugal, Morocco", "Germany, France, Italy"], "answer": "USA, Canada, Mexico"},
    {"q": "How many teams will play in World Cup 2026?", "options": ["32", "48", "24", "64"], "answer": "48"},
    {"q": "When is the FIFA World Cup 2026 final?", "options": ["July 19, 2026", "June 15, 2026", "August 1, 2026", "July 10, 2026"], "answer": "July 19, 2026"},
    {"q": "Where will the World Cup 2026 final be played?", "options": ["MetLife Stadium, New Jersey", "Wembley, London", "Maracana, Brazil", "Santiago Bernabeu, Spain"], "answer": "MetLife Stadium, New Jersey"},
    {"q": "Which country won the last World Cup in 2022?", "options": ["Argentina", "France", "Brazil", "Germany"], "answer": "Argentina"},
    {"q": "How many times has Brazil won the World Cup?", "options": ["5", "4", "3", "2"], "answer": "5"},
    {"q": "Who is the all-time top scorer in World Cup history?", "options": ["Miroslav Klose", "Lionel Messi", "Ronaldo", "Kylian Mbappe"], "answer": "Miroslav Klose"},
]

# Session state
if 'page' not in st.session_state:
    st.session_state.page = 'welcome'
    st.session_state.q_num = 0
    st.session_state.score = 0
    st.session_state.answered = False

# WELCOME PAGE
if st.session_state.page == 'welcome':
    st.title("🎉 Hello! 🎉")
    st.h2("Are you ready for the quiz?")
    st.write("")
    st.write("**This quiz has:**")
    st.write("**5 Questions on Sikkim**")
    st.write("**7 Questions on FIFA World Cup 2026**")
    st.write("**Total: 12 Questions**")
    st.write("")
    if st.button("Let's Start!"):
        st.session_state.page = 'quiz'
        st.rerun()

# QUIZ PAGE
elif st.session_state.page == 'quiz':
    q = questions[st.session_state.q_num]
    
    st.progress((st.session_state.q_num + 1) / 12)
    st.h3(f"Question {st.session_state.q_num + 1} of 12")
    st.write("")
    st.h2(q["q"])
    st.write("")
    
    choice = st.radio("", q["options"], key=st.session_state.q_num)
    
    if st.button("Submit Answer") and not st.session_state.answered:
        st.session_state.answered = True
        if choice == q["answer"]:
            st.success("✅ Right!")
            st.session_state.score += 1
        else:
            st.error(f"❌ Wrong! Correct Answer: {q['answer']}")
        time.sleep(1.5)
        
        if st.session_state.q_num < 11:
            st.session_state.q_num += 1
            st.session_state.answered = False
            st.rerun()
        else:
            st.session_state.page = 'result'
            st.rerun()

# RESULT PAGE
elif st.session_state.page == 'result':
    st.balloons()
    st.title("Quiz Finished! 🎊")
    st.write("")
    st.h2(f"Your Score: {st.session_state.score} / 12")
    st.write("")
    
    if st.session_state.score == 12:
        st.success("🏆 Congratulations! Perfect Score! 👏👏👏")
    elif st.session_state.score >= 10:
        st.success("🌟 Excellent! Well Done! 👏👏")
    elif st.session_state.score >= 6:
        st.info("👍 Good Job!")
    else:
        st.warning("💪 You can try again!")
    
    st.write("---")
    st.h2("Thank You for Playing! ❤️")
    
    if st.button("Play Again"):
        st.session_state.page = 'welcome'
        st.session_state.q_num = 0
        st.session_state.score = 0
        st.session_state.answered = False
        st.rerun()