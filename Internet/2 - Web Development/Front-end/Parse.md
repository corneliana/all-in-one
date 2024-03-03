1. **Tokenization:** the smallest unit of meaning in a language, which could be compared to a word, but more general—can also be numbers, punctuation, or any other individual units in the language.
1. **Parsing Process:** 
	- The parser requests tokens from the lexer (lexical analyzer) one at a time.
	- It attempts to match the sequence of tokens it receives against the syntax rules.
		- Rule matched, a corresponding node is added to the parse tree, and the parser moves on to the next token.
		- No rule matched, the parser may store the token internally (or use other mechanisms, like a stack), and continue to request more tokens.
- The parser keeps going until it finds a rule that matches all the stored tokens or until it determines that there's no valid rule for the sequence of tokens. For example, if the foreign language follows a subject-verb-object (SVO) structure, your brain will try to group words accordingly.
3. **Error Handling:** If, after examining all available tokens, the parser cannot find a valid rule, it raises an exception or reports an error. This indicates that the input does not conform to the expected syntax of the language.