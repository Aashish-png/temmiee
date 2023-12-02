in this I have Used Vue.js 


1.first when the comoponent loads we call the api using onMounted lifecycle hooks of vue.js, I'm storing the data
  in userData also keeping the copy of the data.

2. for sorting I have used a single sort function which takes in parameters it can do sorting in both order ascending 
   and descending but i have only used desending.


3.whenever there is change in end date and if we have valid start date then it shows the data between thoes dates 

4. made the single card comoponeted and looped over it using v-for if you click a remove button then it emits an 
   action from the card comoponet.

5. added keyboard shortcut for filters, you can press tab and selected the filter icon and then press enter to filter 


6.also added the shimmer ui .