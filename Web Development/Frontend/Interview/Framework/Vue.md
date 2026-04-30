**Question 1:** Explain the difference between `v-if` and `v-show`. When would you use each?
- **Domain:** Theoretical Knowledge 
- **Difficulty:** Basic
- **Expected Responses:**
	- `v-if`: Completely adds or removes the element from the DOM.
	- `v-show`: Keeps the element in the DOM and toggles CSS `display: none`.
- **Strong Signals:**
	- Identifies performance trade-offs: `v-if` has higher toggle costs; `v-show` has higher initial render costs.
	- Suggests `v-show` for frequent toggling and `v-if` for rare runtime changes.
- **Negative Signals:**
	- Believes both remove the element from the DOM.


