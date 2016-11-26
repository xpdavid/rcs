``` php
/**
 * get hot questions in current week
 *
 * @return static
 */
public static function getWeeklyQuestions($page, $itemInPage) {
    $question = Cache::remember('week_questions_' . $page . '_' . $itemInPage, 10,
        function() use ($page, $itemInPage) {
        return Question::published()->get()->sortByDesc(
            function($question) use ($page, $itemInPage) {
            $timeDiff = Carbon::parse($question->created_at)->diffInDays(Carbon::now());
            $timeDiff = (30 - $timeDiff) > 0 ? 30 - $timeDiff : 0;
            $numVote = $question->highestVote;
            $numSubscriber = $question->subscribers()->count();
            $numHit_week = $question->hit->week;
            $reward = $question->reward;
            return $numVote + $numSubscriber * 2 + $numHit_week * 5 + $timeDiff * 2 + $reward;
        })->forPage($page, $itemInPage);
    });
    return $question;
}

/**
 * Get hot questions in current month 
 *
 * @return static
 */
public static function getMonthlyQuestions($page, $itemInPage) {
    $question = Cache::remember('month' . $page . '_' . $itemInPage, 10,
        function() use ($page, $itemInPage) {
        return Question::published()->get()->sortByDesc(
            function($question) use ($page, $itemInPage) {
            $timeDiff = Carbon::parse($question->created_at)->diffInDays(Carbon::now());
            $timeDiff = (30 - $timeDiff) > 0 ? 30 - $timeDiff : 0;
            $numVote = $question->highestVote;
            $numSubscriber = $question->subscribers()->count();
            $numHit_month = $question->hit->month;
            $reward = $question->reward;
            return $numVote + $numSubscriber * 2 +
            $numHit_month * 5 + $timeDiff * 2 + $reward;
        })->forPage($page, $itemInPage);
    });

    return $question;
}
```