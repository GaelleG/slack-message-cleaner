# slack-message-cleaner   

Delete Slack conversation messages

## Dirty try on every message   

Run in web browser console:   

```javascript
var overEvent = new MouseEvent("mouseover", {
  bubbles: true,
  cancelable: true,
  view: window
})

var messages = document.querySelectorAll(".c-virtual_list__item")

function deleteMessage(messages, index) {
  console.log("deleteMessage")
  if (index >= messages.length) {
    console.log("index bigger than messages length")
    return
  }

  var message = messages[index]
  message.dispatchEvent(overEvent)
  setTimeout(function() {
    actionsButton = message.querySelector("[data-qa=more_message_actions]")
    if (actionsButton) {
      actionsButton.click()

      setTimeout(function() {
        deleteButton = document.querySelector("[data-qa=delete_message]")
        if (deleteButton) {
          setTimeout(function() {
            deleteButton.click()
            setTimeout(function() {
              confirmButton = document.querySelector("[data-qa=generic_dialog_go]")
              if (confirmButton) {
                confirmButton.click()
              }
              else {
                console.log("no confirmButton")
              }
              setTimeout(function() { deleteMessage(messages, index + 1) }, 100)
            }, 500)
          }, 500)
        }
        else {
          console.log("no deleteButton")
          setTimeout(function() { deleteMessage(messages, index + 1) }, 100)
        }
      }, 100)
    }
    else {
      console.log("no actionsButton")
      setTimeout(function() { deleteMessage(messages, index + 1) }, 100)
    }
  }, 100)
}

deleteMessage(messages, 0)
```

## Delete one user messages  

Set your slack id.   
You can find it on `a.message__sender_link` element.   

```html
<a class="c-message__sender_link" href="/team/HERE" target="_blank" rel="noopener noreferrer" data-stringify-suffix=" ">Bibi</a>
```

```javascript
var USER_TO_CLEAN = "HERE" 
```

Then run this script:   

```javascript
var overEvent = new MouseEvent("mouseover", {
  bubbles: true,
  cancelable: true,
  view: window
})

var deletedNumber = 0

function deleteMessage() {
  console.log("deleteMessage")
  var message = null
  try {
    message = document.querySelector(`a.c-message__sender_link[href="/team/${USER_TO_CLEAN}"]`).parentNode.parentNode.parentNode.parentNode.parentNode
  }
  catch (e) {
    console.error("no more messages - deleted " + deletedNumber)
    return
  }
  message.dispatchEvent(overEvent)
  setTimeout(function() {
    actionsButton = message.querySelector("[data-qa=more_message_actions]")
    if (actionsButton) {
      actionsButton.click()

      setTimeout(function() {
        deleteButton = document.querySelector("[data-qa=delete_message]")
        if (deleteButton) {
          setTimeout(function() {
            deleteButton.click()
            setTimeout(function() {
              confirmButton = document.querySelector("[data-qa=generic_dialog_go]")
              if (confirmButton) {
                confirmButton.click()
                deletedNumber++
              }
              else {
                console.log("no confirmButton")
              }
              setTimeout(function() { deleteMessage() }, 100)
            }, 500)
          }, 500)
        }
        else {
          console.log("no deleteButton")
          setTimeout(function() { deleteMessage() }, 100)
        }
      }, 100)
    }
    else {
      console.log("no actionsButton")
      setTimeout(function() { deleteMessage() }, 100)
    }
  }, 100)
}

deleteMessage()
```
