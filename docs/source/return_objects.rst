Return objects
--------------

Contest
^^^^^^^
Represents general info about one competition.

.. table::

   ============= ===================== ===========================================
   Field         Format                Description
   ============= ===================== ===========================================
   id            integer               Contest ID
   short_name    string                Contest unique short name
   name          string                Name of the contest. May be localized
   hidden        boolean               If true, contest is not visible for public
   rated         boolean               After rating contests, rating is recalculated
   description   string                Description of the contest. May be localized
   difficulty    integer from 1 to 5   Estimated difficulty. 5 is the most difficult
   start_time    integer               Start of the contest (ISO 8601 format)
   duration      integer               Duration of contest in seconds
   status        string                Describes status of the contest

                                       - before
                                       - in_progress
                                       - pending_results
                                       - finished
   has_sandbox   boolean               If true, contest has a sandbox and corresponding API can be used
   frozen_after  integer               May be absent. If contest has sandbox, it will be turned off after number of seconds, set in this variable
   authors       (see example)         Authors of the contest
   managers      (see example)         Managers of the contest
   problems      (see example)         Problems, used in the contest
   ============= ===================== ===========================================

Example
"""""""

.. code-block:: json

   {
      "id": 105,
      "short_name": "tron_contest",
      "name": "First awesome AIForces contest.",
      "hidden": false,
      "rated": true,
      "description": "Lorem ipsum dolor sit amet.",
      "difficulty": 1,
      "start_time": "2019-01-24T23:02:40+00:00",
      "duration": 86400,
      "status": "finished",
      "has_sandbox": true,
      "frozen_after": 82800,
      "authors": {
         "testers": [
            "user_tester_1",
            "user_tester_2",
         ],
         "front-enders": [
            "front-end-god"
         ],
         "problemsetters": [
            "icpc-person"
         ],
         "coordinators": [
            "300iq"
         ]
      },
      "managers": {
         "read": [
            "tester_meshanya",
            "tester_sanya",
         ],
         "write": [
            "problemwriter_anton"
         ]
      },
      "problems": {
         "A": "tron",
         "B": "chess",
         "C": "go"
      }
   }

Tournament
^^^^^^^^^^
Represents general info about one tournament.
(contest may include several intermediate tournament and one final tournament)

.. table::

   ================ ======== ===================================================
   Field            Format   Description
   ================ ======== ===================================================
   id               integer  Tournament ID
   name             string   Name of the tournament. May be localized
   hidden           boolean  If true, tournament is not visible for public
   start_time       integer  Start of the tournament (ISO 8601 format)
   tournament_type  string   One of the built-in tournament types. Check the corresponding documentation
   status           string   Describes status of the tournament

                             - before
                             - in_progress
                             - pending_results
                             - finished
   ================ ======== ===================================================

Example
"""""""

.. code:: json

   {
      "id": 123,
      "name": "first blood",
      "hidden": false,
      "start_time": "2019-01-24T23:02:40+00:00",
      "tournament_type": "swiss",
      "status": "in_progress"
   }

.. _user-label:

User
^^^^
.. table:: User object

   ======================= ========================= ===========================
   Field                   Format                    Description
   ======================= ========================= ===========================
   username                string                    Unique username
   displayname             string                    Display name
   score                   integer/array [#f1]_      | *integer* - a single score
                                                     | *array* - score history array (See :ref:`achievement-label`)
   profile                 object                    | Metadata fields.
                                                     | Their quantity depends on the privacy settings of the user.
                                                     | This field also includes the email and the realname
   participating_contests  array[contest_id]*        List of all contests the user has participated in
   managed_contests        array[contest_id]*        List of all contests the user has priviledged access to
   owned_problems          array[problem_shortname]* List of all the tasks owned by the user
   friends                 array[username]*          List of all the friends the user has
   ======================= ========================= ===========================

.. [#f1] The type of object you receive depends on the query parameters.

Examples
""""""""
Simple object:

.. code:: json

   {
      "username": "coolguy",
      "displayname": "The coolest guy ever 笑",
      "score": 1336,
      "profile": {
         "profile-picture": "https://ourhosting.lol/media/1234beef/coolguy.png"
      }
   }

Complex object:

.. code:: json

   {
      "username": "SuperBoi",
      "displayname": "nеvеr gоnnа givе уоu uр",
      "score": [
         {
            "id": 105,
            "contest": "tron_contest",
            "rating_before": 0,
            "rating_after": 10,
            "problem_scores": {
               "A": 200
            },
            "total_score": 200,
            "user": "SuperBoi",
            "place": 43,
            "participants_number": 60,
            "achieved_at": "2018-11-13T20:20:39+00:00"
         },
         {
            "id": 105,
            "contest": "tron_contest2",
            "rating_before": 10,
            "rating_after": 12,
            "problem_scores": {
               "A": 100,
               "B": 0,
               "C": 0,
               "D": 0
            },
            "total_score": 100,
            "user": "SuperBoi",
            "place": 193,
            "participants_number": 200,
            "achieved_at": "2018-12-21T12:46:34+00:00"
         }
      ],
      "profile": {
         "age": 11,
         "bio": "I love programming",
         "city": "Dublin",
         "country": "Ireland",
         "email": "bestboi09@gmail.cm",
         "gender": "Male",
         "organisation": "Junior High",
         "profile-picture": "https://ourhosting.lol/media/abcdead1/hot_girls.jpg"
      },
      "participating_contests": [ "entry", "easy", "tron1000" ],
      "managed_contests": [ "my_playground" ],
      "owned_problems": [ "SoMuchMoney" ],
      "friends": [ "PiccoloX" ]
   }


Submission
^^^^^^^^^^

Represnts one code submission.

.. table::

   ============= ======== ======================================================
   Field         Format   Description
   ============= ======== ======================================================
   id            integer  Submission ID
   user          string   Username of the person, who made the submission
   contest       string   Contest the submission is attached to
   problem       string   Problem, which solution is presented
   name          string   Short name (may be given by the user to identify submissions in a quick way)
   source_file   string   Name of the source file. File can be downloaded at ``/media/:username/:source_file``
   lang          string   Identifier of the programming language. Read the corresponding documentation
   submitted_at  string   Submission time (ISO 8601 format)
   ============= ======== ======================================================

Example
"""""""

.. code-block:: json

   {
      "id": 278,
      "user": "patrick",
      "contest": "tron_test_contest",
      "problem": "tron",
      "name": "tron_megasolver2000",
      "source_file": "submission_278.py",
      "lang": "pypy3",
      "submitted_at": "2019-01-24T23:02:40+00:00"
   }

.. _achievement-label:

Achievement
^^^^^^^^^^^

Represents one user's participation score in the given contest

.. table::

   ==================== ============== =========================================
   Field                Format         Description
   ==================== ============== =========================================
   id                   integer        Achievement ID
   contest              string         Contest short name
   rating_before        integer        Rating before the contest
   rating_after         integer        Rating after the contest
   problem_scores       object (dict)  Score per problem
   total_score          integer        Sum of scores
   user                 string         Username
   place                integer        Place, that user takes
   participants_number  integer        Contest's participants number
   achieved_at          string         Achievement time (ISO 8601 format)
   ==================== ============== =========================================

Example
"""""""

.. code-block:: json

   {
      "id": 105,
      "contest": "tron_contest",
      "rating_before": 1500,
      "rating_after": 1549,
      "problem_scores": {
         "A": 700,
         "B": 1000,
         "C": 800
      },
      "total_score": 2500,
      "user": "meshanya",
      "place": 3,
      "participants_number": 1000,
      "achieved_at": "2019-01-24T23:02:40+00:00"
   }

Errors
^^^^^^
For every return code >=400 it is guaranteed that the response will contain an
``error`` field explaining the problem.

.. code:: json

   404 NOT FOUND
   {
      "error": "No file with this identifier"
   }
