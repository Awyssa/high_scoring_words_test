# high_scoring_words_test

File name = test_highscoringwords.py

```
import unittest
from highscoringwords import HighScoringWords
import os


class TestHighScoringWords(unittest.TestCase):
    def setUp(self):
        """Initialize the HighScoringWords instance with actual files"""
        # Assuming the files are in the same directory as the test file
        current_dir = os.path.dirname(os.path.abspath(__file__))
        self.wordlist_path = os.path.join(current_dir, "wordlist.txt")
        self.lettervalues_path = os.path.join(current_dir, "letterValues.txt")

        # Verify files exist
        self.assertTrue(os.path.exists(self.wordlist_path), "wordlist.txt not found")
        self.assertTrue(
            os.path.exists(self.lettervalues_path), "letterValues.txt not found"
        )

        # Initialize with actual files
        self.high_scoring = HighScoringWords(
            validwords=self.wordlist_path, lettervalues=self.lettervalues_path
        )

    def test_initialization(self):
        """Test that the class initializes correctly with actual files"""
        # Check that we loaded some words
        self.assertTrue(len(self.high_scoring.valid_words) > 0)

        # Check that we loaded letter values
        self.assertTrue(len(self.high_scoring.letter_values) > 0)

        # Check some specific letter values (adjust these based on your letterValues.txt)
        self.assertIn("a", self.high_scoring.letter_values)
        self.assertIsInstance(self.high_scoring.letter_values["a"], int)

    def test_calculate_word_score(self):
        """Test word score calculation with actual letter values"""
        test_word = self.high_scoring.valid_words[0]
        score = self.high_scoring.calculate_word_score(test_word)

        self.assertIsInstance(score, int)
        self.assertGreaterEqual(score, 0)

        upper_score = self.high_scoring.calculate_word_score(test_word.upper())
        lower_score = self.high_scoring.calculate_word_score(test_word.lower())

        self.assertEqual(upper_score, lower_score)

    def test_build_leaderboard_for_word_list(self):
        """Test building leaderboard from complete word list"""
        leaderboard = self.high_scoring.build_leaderboard_for_word_list()

        # Check leaderboard properties
        self.assertLessEqual(len(leaderboard), HighScoringWords.MAX_LEADERBOARD_LENGTH)
        self.assertGreater(len(leaderboard), 0)

        # Check minimum word length requirement
        for word in leaderboard:
            self.assertGreaterEqual(len(word), HighScoringWords.MIN_WORD_LENGTH)

        # Check ordering (score should be descending)
        for i in range(len(leaderboard) - 1):
            score1 = self.high_scoring.calculate_word_score(leaderboard[i])
            score2 = self.high_scoring.calculate_word_score(leaderboard[i + 1])
            self.assertGreaterEqual(score1, score2)

    def test_build_leaderboard_for_letters(self):
        """Test building leaderboard from given letters"""
        # Use a sample of letters that should form some valid words
        test_letters = "abcdefghijklmnopqrstuvwxyz"  # This should match some words
        leaderboard = self.high_scoring.build_leaderboard_for_letters(test_letters)

        # Check basic properties
        self.assertLessEqual(len(leaderboard), HighScoringWords.MAX_LEADERBOARD_LENGTH)

        # Verify all words can be built from the letters
        for word in leaderboard:
            # Check each word is valid
            self.assertIn(
                word.lower(), [w.lower() for w in self.high_scoring.valid_words]
            )
            # Check minimum length
            self.assertGreaterEqual(len(word), HighScoringWords.MIN_WORD_LENGTH)

        # Test with limited letters
        limited_letters = "abc"
        limited_leaderboard = self.high_scoring.build_leaderboard_for_letters(
            limited_letters
        )
        for word in limited_leaderboard:
            # Check that each letter in the word appears in our limited letters
            word_chars = set(word.lower())
            self.assertTrue(all(c in limited_letters.lower() for c in word_chars))

    def test_max_leaderboard_length(self):
        """Test that leaderboard respects maximum length"""
        word_list_leaderboard = self.high_scoring.build_leaderboard_for_word_list()
        letters_leaderboard = self.high_scoring.build_leaderboard_for_letters(
            "abcdefghijklmnopqrstuvwxyz"
        )

        self.assertLessEqual(
            len(word_list_leaderboard), HighScoringWords.MAX_LEADERBOARD_LENGTH
        )
        self.assertLessEqual(
            len(letters_leaderboard), HighScoringWords.MAX_LEADERBOARD_LENGTH
        )

    def test_empty_and_invalid_inputs(self):
        """Test handling of empty and invalid inputs"""

        # Empty string of letters
        empty_leaderboard = self.high_scoring.build_leaderboard_for_letters("")
        self.assertEqual(len(empty_leaderboard), 0)

        # String with invalid characters
        special_chars_leaderboard = self.high_scoring.build_leaderboard_for_letters(
            "123!@#"
        )
        self.assertEqual(len(special_chars_leaderboard), 0)


if __name__ == "__main__":
    unittest.main()
```
