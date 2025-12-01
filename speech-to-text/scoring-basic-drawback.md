# Scoring - Basic - Drawback

* Phase-2 depended only on keywords, so it failed when the candidate used different words that had the same meaning.
* Example:
  * Expected keyword: “customer handling”
  * Candidate says: “I deal with clients daily and solve their issues.”
  * Meaning is correct, but the exact words “customer” or “handling” do not appear.
  * Phase-2 gives 0 score, even though the answer is fully correct.



