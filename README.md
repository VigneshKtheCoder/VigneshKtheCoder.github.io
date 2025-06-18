import { useState } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';
import { Button } from '@/components/ui/button';
import { Calendar, ChevronLeft, ChevronRight, Flame, Target, Clock } from 'lucide-react';

const StreakCalendar = ({ workouts }) => {
  const [currentDate, setCurrentDate] = useState(new Date());
  
  const monthNames = [
    'January', 'February', 'March', 'April', 'May', 'June',
    'July', 'August', 'September', 'October', 'November', 'December'
  ];

  const getDaysInMonth = (date) => {
    return new Date(date.getFullYear(), date.getMonth() + 1, 0).getDate();
  };

  const getFirstDayOfMonth = (date) => {
    return new Date(date.getFullYear(), date.getMonth(), 1).getDay();
  };

  const getWorkoutsForDate = (date) => {
    const dateString = date.toISOString().split('T')[0];
    return workouts.filter(workout => workout.date.startsWith(dateString));
  };

  const navigateMonth = (direction) => {
    const newDate = new Date(currentDate);
    newDate.setMonth(newDate.getMonth() + direction);
    setCurrentDate(newDate);
  };

  const getDayIntensity = (dayWorkouts) => {
    if (dayWorkouts.length === 0) return 'none';
    if (dayWorkouts.length === 1) return 'light';
    if (dayWorkouts.length === 2) return 'medium';
    return 'high';
  };

  const getDayStyle = (intensity) => {
    switch (intensity) {
      case 'light':
        return 'bg-green-200 text-green-800 border-green-300';
      case 'medium':
        return 'bg-green-400 text-green-900 border-green-500';
      case 'high':
        return 'bg-green-600 text-white border-green-700';
      default:
        return 'bg-gray-50 text-gray-400 border-gray-200';
    }
  };

  const calculateStreak = () => {
    const today = new Date();
    let streak = 0;
    let checkDate = new Date(today);
    
    while (checkDate >= new Date(today.getTime() - 90 * 24 * 60 * 60 * 1000)) {
      const dayWorkouts = getWorkoutsForDate(checkDate);
      if (dayWorkouts.length > 0) {
        streak++;
      } else if (streak > 0) {
        break;
      }
      checkDate.setDate(checkDate.getDate() - 1);
    }
    
    return streak;
  };

  const calculateMonthlyStats = () => {
    const monthStart = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1);
    const monthEnd = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0);
    
    const monthWorkouts = workouts.filter(workout => {
      const workoutDate = new Date(workout.date);
      return workoutDate >= monthStart && workoutDate <= monthEnd;
    });

    const workoutDays = new Set(
      monthWorkouts.map(w => w.date.split('T')[0])
    ).size;

    return {
      totalWorkouts: monthWorkouts.length,
      workoutDays,
      totalCalories: monthWorkouts.reduce((sum, w) => sum + (w.calories || 0), 0),
      totalDuration: monthWorkouts.reduce((sum, w) => sum + w.duration, 0)
    };
  };

  const daysInMonth = getDaysInMonth(currentDate);
  const firstDay = getFirstDayOfMonth(currentDate);
  const currentStreak = calculateStreak();
  const monthStats = calculateMonthlyStats();

  // Create calendar grid
  const calendarDays = [];
  
  // Empty cells for days before the first day of the month
  for (let i = 0; i < firstDay; i++) {
    calendarDays.push(null);
  }
  
  // Days of the month
  for (let day = 1; day <= daysInMonth; day++) {
    const date = new Date(currentDate.getFullYear(), currentDate.getMonth(), day);
    const dayWorkouts = getWorkoutsForDate(date);
    const intensity = getDayIntensity(dayWorkouts);
    
    calendarDays.push({
      day,
      date,
      workouts: dayWorkouts,
      intensity
    });
  }

  return (
    <div className="space-y-6">
      {/* Streak Stats */}
      <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
        <Card className="bg-gradient-to-r from-orange-50 to-orange-100 border-orange-200">
          <CardContent className="p-4">
            <div className="flex items-center justify-between">
              <div>
                <p className="text-orange-600 text-sm font-medium">Current Streak</p>
                <p className="text-2xl font-bold text-orange-800">{currentStreak}</p>
                <p className="text-orange-600 text-xs">days</p>
              </div>
              <Flame className="h-8 w-8 text-orange-500" />
            </div>
          </CardContent>
        </Card>

        <Card className="bg-gradient-to-r from-blue-50 to-blue-100 border-blue-200">
          <CardContent className="p-4">
            <div className="flex items-center justify-between">
              <div>
                <p className="text-blue-600 text-sm font-medium">This Month</p>
                <p className="text-2xl font-bold text-blue-800">{monthStats.totalWorkouts}</p>
                <p className="text-blue-600 text-xs">workouts</p>
              </div>
              <Target className="h-8 w-8 text-blue-500" />
            </div>
          </CardContent>
        </Card>

        <Card className="bg-gradient-to-r from-green-50 to-green-100 border-green-200">
          <CardContent className="p-4">
            <div className="flex items-center justify-between">
              <div>
                <p className="text-green-600 text-sm font-medium">Active Days</p>
                <p className="text-2xl font-bold text-green-800">{monthStats.workoutDays}</p>
                <p className="text-green-600 text-xs">this month</p>
              </div>
              <Calendar className="h-8 w-8 text-green-500" />
            </div>
          </CardContent>
        </Card>

        <Card className="bg-gradient-to-r from-purple-50 to-purple-100 border-purple-200">
          <CardContent className="p-4">
            <div className="flex items-center justify-between">
              <div>
                <p className="text-purple-600 text-sm font-medium">Total Hours</p>
                <p className="text-2xl font-bold text-purple-800">{Math.round(monthStats.totalDuration / 60)}</p>
                <p className="text-purple-600 text-xs">this month</p>
              </div>
              <Clock className="h-8 w-8 text-purple-500" />
            </div>
          </CardContent>
        </Card>
      </div>

      {/* Calendar */}
      <Card>
        <CardHeader>
          <div className="flex items-center justify-between">
            <CardTitle className="flex items-center gap-2">
              <Calendar className="h-5 w-5" />
              Workout Calendar
            </CardTitle>
            <div className="flex items-center gap-2">
              <Button variant="outline" size="sm" onClick={() => navigateMonth(-1)}>
                <ChevronLeft className="h-4 w-4" />
              </Button>
              <span className="font-medium min-w-[140px] text-center">
                {monthNames[currentDate.getMonth()]} {currentDate.getFullYear()}
              </span>
              <Button variant="outline" size="sm" onClick={() => navigateMonth(1)}>
                <ChevronRight className="h-4 w-4" />
              </Button>
            </div>
          </div>
        </CardHeader>
        <CardContent>
          {/* Calendar Grid */}
          <div className="grid grid-cols-7 gap-2 mb-4">
            {['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'].map(day => (
              <div key={day} className="text-center text-sm font-medium text-muted-foreground p-2">
                {day}
              </div>
            ))}
            
            {calendarDays.map((day, index) => (
              <div key={index} className="aspect-square">
                {day ? (
                  <div 
                    className={`w-full h-full border-2 rounded-lg flex flex-col items-center justify-center cursor-pointer transition-all hover:scale-105 ${getDayStyle(day.intensity)}`}
                    title={day.workouts.length > 0 ? 
                      `${day.workouts.length} workout${day.workouts.length > 1 ? 's' : ''} on ${day.date.toLocaleDateString()}` : 
                      'No workouts'
                    }
                  >
                    <span className="text-sm font-medium">{day.day}</span>
                    {day.workouts.length > 0 && (
                      <span className="text-xs">
                        {day.workouts.length}
                      </span>
                    )}
                  </div>
                ) : (
                  <div className="w-full h-full" />
                )}
              </div>
            ))}
          </div>

          {/* Legend */}
          <div className="flex items-center justify-center gap-4 text-sm">
            <div className="flex items-center gap-2">
              <div className="w-4 h-4 bg-gray-50 border-2 border-gray-200 rounded"></div>
              <span className="text-muted-foreground">No workout</span>
            </div>
            <div className="flex items-center gap-2">
              <div className="w-4 h-4 bg-green-200 border-2 border-green-300 rounded"></div>
              <span className="text-muted-foreground">1 workout</span>
            </div>
            <div className="flex items-center gap-2">
              <div className="w-4 h-4 bg-green-400 border-2 border-green-500 rounded"></div>
              <span className="text-muted-foreground">2 workouts</span>
            </div>
            <div className="flex items-center gap-2">
              <div className="w-4 h-4 bg-green-600 border-2 border-green-700 rounded"></div>
              <span className="text-muted-foreground">3+ workouts</span>
            </div>
          </div>
        </CardContent>
      </Card>

      {/* Streak Achievements */}
      <Card>
        <CardHeader>
          <CardTitle>Streak Milestones</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
            {[
              { milestone: 3, emoji: '🔥', title: 'Getting Started', achieved: currentStreak >= 3 },
              { milestone: 7, emoji: '💪', title: 'One Week Strong', achieved: currentStreak >= 7 },
              { milestone: 14, emoji: '⭐', title: 'Two Week Warrior', achieved: currentStreak >= 14 },
              { milestone: 30, emoji: '🏆', title: 'Monthly Master', achieved: currentStreak >= 30 }
            ].map((achievement) => (
              <div
                key={achievement.milestone}
                className={`text-center p-4 rounded-lg border-2 transition-all ${
                  achievement.achieved
                    ? 'bg-gradient-to-r from-green-50 to-green-100 border-green-300'
                    : 'bg-gray-50 border-gray-200'
                }`}
              >
                <div className="text-3xl mb-2">{achievement.emoji}</div>
                <p className={`font-medium ${achievement.achieved ? 'text-green-800' : 'text-gray-500'}`}>
                  {achievement.title}
                </p>
                <p className={`text-sm ${achievement.achieved ? 'text-green-600' : 'text-gray-400'}`}>
                  {achievement.milestone} day streak
                </p>
                {achievement.achieved && (
                  <Badge className="mt-2 bg-green-500 text-white">Achieved!</Badge>
                )}
              </div>
            ))}
          </div>
        </CardContent>
      </Card>
    </div>
  );
};

export default StreakCalendar;
