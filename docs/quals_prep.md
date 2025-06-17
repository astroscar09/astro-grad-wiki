# Qualifying Exam Prep Materials

Preparing for the qualifying exam can feel daunting, but this page is here to make it easier. Below you'll find resources curated by graduate students to help you understand what to expect and practice with real questions from past exams.

We hope this page serves as a useful hub for everyone preparing for the second year qualifying exam.

---

## What to Expect

Here is a general guide:

- At the beginning of the Sping semester the seminar leader should get in contact with you to decide when you will be giving your Quals Talk
- Once you decide on the date for your talk you should talk with with committee to solidify the date for your closed door session. 

*Note*: You are in charge of reserving the room for the closed door session so better to do this earlier rather than later.

- **4 weeks** out you will recieve your papers to read and prep for the closed door session
- **2 weeks** out you should send your second year report to your committee members
- **1 week** out the Grad Reps will get in contact with you about having mock quals sessions to prepare you

## What to do
- You are expected to give a **45 minute** talk about your second year project. You get to choose when this happens but it is typically in one of the Seminars(Exgal, Stellar, Cosmology). 
- After your talk you need to do a **1 hour** closed door session. Typically these are done immediately after your Quals talk but you can have the close door session at a different day if needed. 
- During the closed door session expect to be asked about your Quals talk, your research and connecting the papers you have read with the broader scope of your research.

*(If you'd like to contribute to or update the guide, please contact the Department Archivist.)*

---

## Past Qualifying Exam Questions

This is a living archive of past questions asked during qualifying exams. Questions are tagged by professor and topic. Use the filters in the embedded sheet below to browse or search.

---

<table id="qual-table" class="display" style="width:100%">
  <thead>
    <!-- Row 0: Filter dropdowns -->
    <tr>
      <th><select><option value="">All</option></select></th>
      <th><select><option value="">All</option></select></th>
      <th><select><option value="">All</option></select></th>
      <th></th>
    </tr>
    <!-- Row 1: Actual column headers -->
    <tr>
      <th>Year</th>
      <th>Professor</th>
      <th>Topic</th>
      <th>Question</th>
    </tr>
  </thead>
</table>

<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
<script>
  fetch('../quals_files/questions.csv')
    .then(response => response.text())
    .then(csvText => {
      // Parse CSV with header:true so first row is header, not data
      const parsed = Papa.parse(csvText, { header: true });
      const data = parsed.data.filter(row => Object.values(row).some(cell => cell.trim() !== ""));

      const table = $('#qual-table').DataTable({
        data: data,
        columns: [
          { data: "Year" },
          { data: "Professor" },
          { data: "Topic" },
          { data: "Question" }
        ],
        orderCellsTop: true,     // needed when filters are in the header
        fixedHeader: false,      // disable fixed header to avoid duplicate rows
        initComplete: function () {
          var api = this.api();

          // For each column except last (Question), populate the filter dropdowns in the first header row (index 0)
          api.columns().every(function (colIdx) {
            if (colIdx === 3) return; // skip Question column

            var column = this;
            var select = $('thead tr:eq(0) th').eq(colIdx).find('select');

            // Get unique sorted values and append to the dropdown
            column.data().unique().sort().each(function (d) {
              if (d) {
                select.append('<option value="' + d + '">' + d + '</option>');
              }
            });

            // Filter on dropdown change
            select.on('change', function () {
              var val = $.fn.dataTable.util.escapeRegex($(this).val());
              column.search(val ? '^' + val + '$' : '', true, false).draw();
            });
          });
        }
      });
    });
</script>
