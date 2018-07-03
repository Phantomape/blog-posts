---
title: An interesting bug(Draft)
date: 2018-07-02 19:08:04
tags: 
- React
categories: Frontend
---
#   Original Code
Let's take a look at the code snippet
```
    calculate() {
        console.log('Call calculate()');
        const isCompleted = ...;

        if (isCompleted) {
            const answers = ...;
            this.fetchWhatWeWant(answers);
        } else {
            console.log('Before setting latestQuery to null');
            this.latestTotalPriceQuery = null;
            console.log('After setting latestQuery to null');
        }
    }

    // This fetchWhatWeWant function is already debounced
    fetchWhatWeWant(answers) {
        if (itemPriceAnswers === null) {
            this.latestQuery = null;
            return Promise.resolve();
        }

        const endpoint = ...;
        const payload = ...;

        const query = postJson(endpoint, payload);
        query.then((response) => {
            if (!_.isEqual(query, this.latestQuery)) {
                return Promise.resolve();
            }
            console.log('Comparing');
            console.log(this.latestQuery);
            console.log(query);
            ...
            return Promise.resolve();
        })
        .catch(() => {
            if (_.isEqual(query, this.latestQuery)) {
               ...
            }
        })
        .then(() => {
            if (_.isEqual(query, this.latestQuery)) {
                this.latestQuery = null;
            }
        });

        console.log('Before setting latestTotalPriceQuery to query');
        this.latestTotalPriceQuery = query;
        console.log('After setting latestTotalPriceQuery to query');
        return query;
    }
```
We thought the code could invalidate the late response that comes from the server, unfortunately, it can't it still take the last response, which is invalid data, as the valid result.

#   Log Results
```
3x Call calculate()
Before setting latestTotalPriceQuery to null
After setting latestTotalPriceQuery to null
Before setting latestTotalPriceQuery to query
After setting latestTotalPriceQuery to query
Comparing
Promise { <state>: "fulfilled", <value>: {…} }
Promise { <state>: "fulfilled", <value>: {…} }
```
This is the log result of the original code, we could see that the reason why the code doesn't work is because the call chain doesn't meet our expectations. For example, when we reduce the input from 123 to nothing by hitting the backspace nonstoppingly, the call chain is like 
```
3x calls to calculate() finished -> setting the latestQuery to be null -> fetchWhatWeWant(1) -> setting the latestQuery to be query -> recv the response from server
```
We can see that this bug is mainly due the debounce nature of ```fetchWhatWeWant()```, cause there is no guarantee it was executed in the last call of ```calculate()```