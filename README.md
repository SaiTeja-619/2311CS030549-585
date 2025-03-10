2311CS030549-585
AlogoQuest implements a quiz console using a queue and stack.

class Question:
    def __init__(self, text, options, correct_option):
        self.text = text
        self.options = options
        self.correct_option = correct_option
        self.next = None

class QuizQueue:
    def __init__(self):
        self.front = self.rear = None

    def add_question(self, text, options, correct_option):
        new_question = Question(text, options, correct_option)
        if not self.rear:
            self.front = self.rear = new_question
        else:
            self.rear.next = new_question
            self.rear = new_question

    def get_next_question(self):
        if not self.front:
            return None
        temp = self.front
        self.front = self.front.next
        if not self.front:
            self.rear = None
        return temp

class AnswerStack:
    def __init__(self):
        self.answers = []

    def save_answer(self, answer):
        self.answers.append(answer)

    def get_answers(self):
        return self.answers

def run_quiz(quiz, answers):
    current_question = quiz.front
    while current_question:
        print(f"\n{current_question.text}")
        for option in current_question.options:
            print(option)
        user_answer = input("Enter your answer (A/B/C/D): ").strip().upper()
        answers.save_answer(user_answer)
        current_question = current_question.next

def display_score(answers, quiz):
    score = 0
    current_question = quiz.front
    for user_answer in answers.get_answers():
        if current_question and user_answer == current_question.correct_option:
            score += 1
        current_question = current_question.next
    print(f"\nYour Final Score: {score}/{len(answers.get_answers())}")

if __name__ == "__main__":
    quiz = QuizQueue()
    answers = AnswerStack()

    quiz.add_question("Which fruit starts with 'A'?", ["A. Apple", "B. Banana", "C. Cherry", "D. Date"], 'A')
    quiz.add_question("Which animal barks?", ["A. Cat", "B. Dog", "C. Elephant", "D. Fish"], 'B')
    quiz.add_question("What is the capital of France?", ["A. Berlin", "B. Madrid", "C. Paris", "D. Rome"], 'C')
    quiz.add_question("How many continents are there?", ["A. 5", "B. 6", "C. 7", "D. 8"], 'C')
    quiz.add_question("Which planet is known as the Red Planet?", ["A. Earth", "B. Venus", "C. Mars", "D. Jupiter"], 'C')

    run_quiz(quiz, answers)
    display_score(answers, quiz)
