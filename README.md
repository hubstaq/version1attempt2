<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Excel-like Spreadsheet</title>
  <style>
    body {
      margin: 0;
      padding: 20px;
      font-family: Arial, sans-serif;
    }
    table {
      border-collapse: collapse;
      margin: auto;
    }
    td {
      width: 50px;
      height: 30px;
      border: 1px solid #ccc;
      text-align: center;
      vertical-align: middle;
      position: relative;
    }
    td:hover {
      background-color: yellow;
    }
    td.clickable {
      cursor: pointer;
    }
    td.center-cell {
      background-color: #f0f0f0;
      font-weight: bold;
      cursor: default;
    }
    /* Modal styles */
    .modal {
      display: none; /* Hidden by default */
      position: fixed;
      z-index: 1;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background-color: rgba(0,0,0,0.4);
    }
    .modal-content {
      background-color: #fff;
      margin: 15% auto;
      padding: 20px;
      border: 1px solid #888;
      width: 80%;
      max-width: 500px;
      position: relative;
    }
    .close {
      color: #aaa;
      position: absolute;
      top: 10px;
      right: 15px;
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
    }
    .close:hover,
    .close:focus {
      color: black;
    }
  </style>
</head>
<body>
  <div id="tableContainer"></div>

  <!-- Modal Popup -->
  <div id="popupModal" class="modal">
    <div class="modal-content">
      <span class="close" id="closeModal">&times;</span>
      <div id="popupContent"></div>
    </div>
  </div>

  <script>
    // Configuration
    const numRows = 25;
    const numCols = 20;
    // Determine center cell (using 0-index: center row = floor(25/2), center col = floor(20/2))
    const centerRow = Math.floor(numRows / 2);
    const centerCol = Math.floor(numCols / 2);

    // Generate popup content for each clickable cell (total cells minus the one center cell)
    const totalClickable = numRows * numCols - 1;
    const popups = [];
    for (let i = 0; i < totalClickable; i++) {
      if (i % 4 === 0) {
        popups.push("Word " + (i + 1));
      } else if (i % 4 === 1) {
        popups.push("<a href='https://example.com/page" + (i + 1) + "' target='_blank'>Link " + (i + 1) + "</a>");
      } else if (i % 4 === 2) {
        popups.push("This is a phrase number " + (i + 1) + " - inspiring!");
      } else {
        popups.push("<img src='https://picsum.photos/seed/" + (i + 1) + "/100/100' alt='Random image " + (i + 1) + "' />");
      }
    }

    // Build the table dynamically
    const tableContainer = document.getElementById("tableContainer");
    const table = document.createElement("table");
    let popupIndex = 0; // to assign a unique popup to each clickable cell

    for (let r = 0; r < numRows; r++) {
      const tr = document.createElement("tr");
      for (let c = 0; c < numCols; c++) {
        const td = document.createElement("td");
        // Check if this is the center cell
        if (r === centerRow && c === centerCol) {
          td.innerHTML = "HUBSTAQ";
          td.classList.add("center-cell");
        } else {
          td.classList.add("clickable");
          // Capture current popup index for this cell using a closure
          let currentPopup = popupIndex;
          td.addEventListener("click", function() {
            showPopup(popups[currentPopup]);
          });
          popupIndex++;
        }
        tr.appendChild(td);
      }
      table.appendChild(tr);
    }
    tableContainer.appendChild(table);

    // Modal functions
    const modal = document.getElementById("popupModal");
    const popupContent = document.getElementById("popupContent");
    const closeModal = document.getElementById("closeModal");

    function showPopup(content) {
      popupContent.innerHTML = content;
      modal.style.display = "block";
    }

    closeModal.onclick = function() {
      modal.style.display = "none";
    };

    window.onclick = function(event) {
      if (event.target == modal) {
        modal.style.display = "none";
      }
    };
  </script>
</body>
</html>
