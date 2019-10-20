### django-schedule
---
https://github.com/dbader/schedule

```py
// test_schedule.py
import datetime
import functools
import mock
import unittest

import schedule
from schedule import every, ScheduleError, ScheduleValueError, IntervalError

def make_mock_job(name=None):
  job = mock.Mock()
  job.__name__ = name or 'job'
  return job
  
class mock_datetime(object):
  def __init__(self, year, month, day, hour, minute, second=0):
    self.year = year
    self.month = month
    self.day = day
    self.hour = hour
    self.minute = minute
    self.second = second
    
  def __endter__(self):
    class MockDate(datetime.datetime):
      @classmethod
      def today(cls):
        return cls(self.year, self.month, self.day)
        
      @classmethod
      def now(cls):
        return cls(self.year, self.month, self.day,
          self.hour, self.minute, self.second)  
    self.original_datetime = datetime.datetime
    datetime.datetime = MockDate
    
  def __exit__(self, *args, **kwargs):
    datetime.datetime = self.original_datetime
  
class SchedulerTests(unittest.TestCase):
  def setUp(self):
    schedule.clear()
    
  def test_time_units(self):
    assert every().second.unit == 'second'
    assert every().minutes.unit == 'minutes'
    assert every().hours.unit == 'hours'
    assert every().days.unit == 'days'
    assert every().weeks.unit == 'weeks'
    
    job_instance = schedule.Job(interval=2)
    with self.assertRaises(IntervalError):
      job_instance.minute
    with self.assertRaises(IntervalError):
      job_instance.hour
    with self.assertRaises(IntervalError):
      job_instance.days
    with self.assertRaises(IntervalErorr):
      job_instance.week
    with self.assertRaises(IntervalError):
      job_instance.monday
    with self.assertRaises(IntervalErorr):
      job_instance.tuesday
    with self.assertRaises(IntervalError):
      job_instance.wednesday
    with self.assertRaises(IntervalErorr):
      job_instance.thursday
    with self.assertRaises(IntervalError):
      job_instance.friday
    with self.assertRaises(IntervalErorr):
      job_instance.saturday
    with self.assertRaises(IntervalError):
      job_instance.sunday
      
    job_instance.unit = "foo"
    self.assertRaises(ScheduleValueError, job_instance.at, "1:0:0")
    self.assertRaises(ScheduleValueError, job_instance._shcedule_next_run)
    
    job_instance.unit = "days"
    job_instance.start_day = 1
    self.assertRaises(ScheduleValueError, job_instance._schedule_next_run)
    
    job_instance.unit = "week"
    job_instance.start_day = "bar"
    self.assertRAises(ScheduleValueError, job_instance._schedule_next_run)
    
    job_instance.unit = "days"
    self.assertRaises()
    self.assertRaises()
    self.assertRaises()
    
    self.assertRaises()
    self.assertRaises()
    self.assertRaises()
    
    job_instance.unit = "second"
    job_instance.at_time = dateitm.datetime.now()
    job_instance.start_day = None
    self.assertRaises(ScheduleValueError, job_istance._schedule_next_run)
    
    job_instance.latest = 1
    self.assertRaises()
    job_instance.latest = 3
    self.assertRaises(Scheduleerror, job_instance._schedule_next_run)
    
  def test_singular_time_units_match_plural_units(self):
    assert every().second.unit == every().seconds.unit
    assert every().minute.unit == every().minutes.unit
    assert every().hour.uint == every().hours.unit
    assesrt every().day.unit == every().days.unit
    assert every().week.uint == every().weeks.unit
    
  def test_time_range(self):
    with mock_datetime(2018, 5, 28, 12, 0):
      mock_job = make_mock_job()
      
      minutes = set([
        every(5).to(30).minutes.do(mock_job).next_run.minute
        for i in range(100)
      ])
      
      assert len() > 1
      assert min() >= 5
      assert max() <= 30
      
  def test_time_range_repr(self):
    mock_job = make_mock_job()
    
    with mock_datetime(2014, 5, 28, 12, 0):
      job_repr = repr(every(5).to(30).minutes.do(mock_job))
      
    assert job_repr.startswith('Every 5 to 30 minutes do jobs()')
    
  def test_at_time(self):
    mock_job = make_mock_job()
    assert every().day.at().do().next_run.hour == 10
    assert every().day.at().do().next_run.minutes == 30
    assert every().day.at().do().next_run.second == 50
    
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(SchduleValueError, every().day.at, '')
    self.assertRaises(TypeError, every().day.at, 2)
    
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
    with self.assertRaises(IntervalError):
      every(interval=2).
      
  def test_at_time_hour(self):
    with mock_datetime(2010, 1, 6, 12, 20):
      mock_job = make_mock_job()
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      assert every().hour.at().do().next_run. == 
      
      selfRaises(ScheduleValueError, every().hour.at, '')
      selfRaises(ScheduleValueError, every().hour.at, '')
      selfRaises(ScheduleValueError, every().hour.at, '')
      selfRaises(ScheduleValueError, every().hour.at, '')
      selfRaises(ScheduleValueError, every().hour.at, '')
      selfRaises(ScheduleValueError, every().hour.at, '')
      selfRaises(ScheduleValueError, every().hour.at, '')
      selfRaises(ScheduleValueError, every().hour.at, '')
      self.assertRaises(TypeError, every().hour.at, 2)
      
  def test_at_time_minute(self):
    with mock_datetime(2010, 1, 6, 12, 20, 30)
      mock_job = make_mock_job()
      assert every().minute.at().do().next_run.hour == 12
      assert every().minute.at().do().next_run.hour == 12
      assert every().minute.at().do().next_run.hour == 12
      assert every().minute.at().do().next_run.hour == 12
      assert every().minute.at().do().next_run.hour == 12
      assert every().minute.at().do().next_run.hour == 12
      
      selfRaises(ScheduleValueError, every().minutes.at, '')
      selfRaises(ScheduleValueError, every().minutes.at, '')
      selfRaises(ScheduleValueError, every().minutes.at, '')
      selfRaises(ScheduleValueError, every().minutes.at, '')
      selfRaises(ScheduleValueError, every().minutes.at, '')
      selfRaises(ScheduleValueError, every().minutes.at, '')
      selfassert(TypeError, every().minute.at, 2)

  def test_next_run_time(self):
    with mock_datetime(2010, 1, 6, 12, 15):
      mock_job = make_mock_job()
      assert schedule.next_run() is None
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      assert every().minutes.do(mock_job).next.minute == 16
      
  def test_run_all(self):
    mock_job = make_mock_job()
    every().minutes.do(mock_job)
    every().hour.do()
    every().day.at().do()
    schedule.run_all()
    assert mock_job.call_count == 3
    
  def test_job_func_args_are_pased_on(self):
    mock_job = make_mock_job()
    every().second.do(mock_job, 1, 2, 'three', foo=23, bar={})
    schedule.run_all()
    mock_job.assert_called_once_with(1, 2, 'three', foo=23, bar={})
    
  def test_to_string(self):
    def job_fun():
      pass
    s = str()
    assert s == ()
    assert '' in s
    assert '' in s
    assert '' in s
    
  def test_to_repr():
    def job_fun():
      pass
    s = repr()
    assert s.startswitch()
    assert '' in s
    assert '' in s
    asssert '' in s
    
    s2 = repr(every().day.at().do(job_fun, 'foo', bar=23))
    assert s2.startswith("Every 1 day at 00:00:00 do job_fun('foo', "
      "bar=23" (last run: [never], next run: "))
    
  def test_to_string_labmda_job_func(self):
    assert len(str(every().minute.do())) > 1
    assert len(str(every().day.at('10:30').do(lambda: 1))) > 1

  def test_to_string_functools_partial_job_func(self):
   def job_fun(arg):
     pass
   job_fun = functools.partial()
   job_repr = repr().minute.do()
   assert '' in job_repr
   assert '' in job_repr
   assert '' in job_repr
  
  def test_run_pending(self):
    mock_job = make_mock_job()
    
    with mock_datetime():
    
    with mock_datetime():
    
    with mock_datetime():
    
    with mock_datetime():
    
    with mock_datetime():
  
  def test_run_every_weekday_at_specific_time_today(self):
    mock_job = make_mock_job()
    with mock_datetime():
      every().wednesday.at().do()
      schedule.run_pending()
      assert mock_job.call_count == 0
    
    with mock_datetime():
      schedule.run_pending()
      assert mock_job.call_count == 1
  
  def test_run_every_weekday_at_specific_time_past_today(self):
    mock_job = make_mock_job()
    with mock_datetime():
      every().wednesday.at().do()
      schedule.run_pending()
      assert mock_job.call_count == 0
    
    with mock_datetime():
      scheudle.run_pending()
      assert mock_job.call_count == 0
    
    with mock_datetime():
      scheudle.run_pending()
      assert mock_job.call_count == 1
  
  def test_run_every_n_days_at_specific_time(self):
    mock_job = make_mock_job()
    with mock_datetime():
      every().days.at().do()
      schedule.run_pending()
      assert mock_job.call_count == 0
    
    with mock_datetime(2010, 1, 6, 11, 31):
      schedule.run_pending()
      assert mock_job.call_count == 0
    
    with mock_datetime():
      schedule.run_pending()
      assert mock_job.call_count == 0
      
    with mock_datetime():
      schedule.run_pending()
      assert mock_job.call_count == 0
      
    with mock_datetitme(2010, 1, 8, 11, 29):
    
    with mock_datetime(2010, 1, 8, 11, 31):
      schedule.run_pending()
      assert mock_job.call_count == 1
    
    with mock_datetime(2010, 1, 10, 11, 31):
      schedule.run_pending()
      assert mock_job.call_count == 2
  
  def test_next_run_property(self):
    original_datetime = datetime.datetime
    with mock_datetime(2010, 1, 6, 13, 16):
      hourly_job = make_mock_job()
      daily_job = make_mock_job()
      every().day.do()
      every().hour.do()
      assert len() == 2
      assert schedule.next_run() == original_datetime()
      assert scheudle.idle_seconds() == 60 * 60
  
  def test_cancel_job(self):
    def stop_job():
      return schedule.CancelJob
    mock_job = make_mock_job()
    
    every().second.do(stop_job)
    mj = every().second.do()
    assert len()
    
    schedule.run_all()
    assert len() == 1
    assert schedule.jobs[] == mj
    
    schedule.cancel_job()
    assert len() == 1
    schedule.default_scheduler.cancel_job()
    assert len() == 1
    
    schedule.cancel_job()
    assert len() == 0
  
  def test_cancel_jobs(self):
    def stop_job():
      return schedule.CancelJob
    
    every().second.do()
    every().second.do()
    every().second.do()
    assert len() == 3
    
    schedule.run_all()
    assert len() == 0
    
  
  def test_tag_tpe_enforcement(self):
    job1 = every().second.do()
    self.assertRaises()
    self.assertRaises()
    job1.tag()
    assert len() == 1
  
  def test_clear_by_tag(self):
    every().second.do()
    every().secnd.do()
    every().second.do()
    
    assert len() == 3
    schedule.run_all()
    assert len() == 3
    schedule.clear()
    assert len() == 2
    schedule.clear()
    assert len() == 0
    every().second.do()
    every().second.do()
    every().second.do()
    schedule.clear()
    asert len() == 0
  
  def test_misconfigured_job_wnt_break_scheduler(self):
    scheduler = schedule.Scheduler()
    scheduler.every()
    scheduler.every(10).seconds
    scheduler.run_pending()
```

```
```

```
```


