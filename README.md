# high_scoring_words_test

File name = test_highscoringwords.py

```
import unittest
from highscoringwords import HighScoringWords

class TestHighScoringWordsWithMocks(unittest.TestCase):
    def setUp(self):
        self.mock_words = ["lyst", "fashion", "geek","geektastic", "stylish", "london"]
        self.mock_letter_values = {
            "a": 1, "b": 3, "c": 3, "d": 2, "e": 1, "f": 4, "g": 2,
            "h": 4, "i": 1, "j": 8, "k": 5, "l": 1, "m": 3, "n": 1,
            "o": 1, "p": 3, "q": 10, "r": 1, "s": 1, "t": 1, "u": 1,
            "v": 4, "w": 4, "x": 8, "y": 4, "z": 10
        }
        self.high_scoring = HighScoringWords()
        self.high_scoring.valid_words = self.mock_words
        self.high_scoring.letter_values = self.mock_letter_values

    def test_calculate_word_score(self):
        """It should test that the given word returns the correct score"""
        word = "dictionary"
        expected_score = sum(self.mock_letter_values[letter] for letter in word)
        score = self.high_scoring.calculate_word_score(word)
        self.assertEqual(score, expected_score)

    def test_build_leaderboard_for_word_list(self):
        """It should build the leaderboard of the top valid words"""
        leaderboard = self.high_scoring.build_leaderboard_for_word_list()
        expected_leaderboard = ['geektastic', 'fashion', 'stylish', 'geek', 'london', 'lyst']
        self.assertEqual(leaderboard, expected_leaderboard)

    def test_build_leaderboard_for_letters(self):
        """It should test building the leaderboard from the given starting_letters"""
        test_letters = "lygstfaee18hioknd"
        leaderboard = self.high_scoring.build_leaderboard_for_letters(test_letters)
        expected_leaderboard = ["fashion", "geek", "lyst"]
        self.assertEqual(leaderboard, expected_leaderboard)

        test_letters = "styonl245ish*(don!" # Should also work with special characters
        leaderboard = self.high_scoring.build_leaderboard_for_letters(test_letters)
        expected_leaderboard = ["stylish", "london", "lyst"]
        self.assertEqual(leaderboard, expected_leaderboard)

        special_chars_leaderboard = self.high_scoring.build_leaderboard_for_letters("123!@#")
        self.assertEqual(len(special_chars_leaderboard), 0)

    def test_max_leaderboard_length(self):
        """It should test that leaderboard use the maximum length"""
        leaderboard = self.high_scoring.build_leaderboard_for_word_list()
        self.assertLessEqual(len(leaderboard), HighScoringWords.MAX_LEADERBOARD_LENGTH)

if __name__ == "__main__":
    unittest.main()
```
