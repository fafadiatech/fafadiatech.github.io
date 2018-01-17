---
layout: post
title:  "Download NSE & BSE Bhavcopies in Python using Click"
date:   2018-01-16 00:00:00 +0530
categories: python script
---

One of the key selling points of Python is ability to create command line script that have ability to automate some mundane tasks. [Bhav Copies](https://www.quora.com/What-are-the-BHAV-copy-reports-of-NSE) are equivalent to data feeds we get from exchanges. We wanted to demonstrate power of simple scripts, so we chose to write a script to download data feeds from BSE and NSE. 

For complete code [review this python script](https://raw.githubusercontent.com/fafadiatech/ft-learnings/master/python/click/download_bhavcopy.py), the heart of the code is listed as below:

```python
@click.command()
@click.argument(
    "exchange",
    default="nse")
@click.option(
    "--for_date",
    default=yesterday(),
    help="Date for which to download bhavcopy DD/MM/YYYY format")
@click.option(
    "--for_past_days",
    default=1,
    help="Number of calendar days for which we need to fetch data {E.g. past 15 days from today}")
def main(exchange, for_date, for_past_days):
    """
    download_bhavcopy is utility that will download daily bhav copies
    from NSE and BSE

    Examples:
    python download_bhavcopy.py bse --for_date 06/12/2017

    python download_bhavcopy.py bse --for_past_days 15
    """
    click.echo(f"downloading bhavcopy {exchange}")

    # We need to fetch data for past X days
    if for_past_days != 1:
        for i in range(for_past_days):
            ts = datetime.now() - timedelta(days=i+1)
            ts = ts.strftime("%d/%m/%Y")
            if exchange == "nse":
                download_nse_bhavcopy(ts)
            else:
                download_bse_bhavcopy(ts)
    else:
        if exchange == "nse":
            download_nse_bhavcopy(for_date)
        else:
            download_bse_bhavcopy(for_date)
```

Key things to note:

1. You can take any python function and decorate it with `@click.command` to make it behave like a command line interface. What this means is you get `--help` flag out of the box.
1. Click has two key concepts around parameters going into `main` function here:
    1. Argument: These are mandatory. Look at how we've made `exchange` a mandatory parameter
    2. Options: These are parameters/switch that can or cannot be present. Both `for_date` and `for_days` are example of optional arguments
1. The document string that you write below the function that you've decorated using `@click.command` will be printed when you invoke the command with `--help` flag. We believe this is one of the most awesome features
1. With optional arguments you can specify `help` which also produces nicer output
1. You can also use `default` argument in `@click.option` decorator to specify default values. This is also used to infer data-type of argument. Thus we don't need to do all parsing and validation.
1. Click also has [Sub Commands](http://click.pocoo.org/6/commands/) this make grouping and implementing sub-commands real easy

```sh
(click_demo) Sidharths-MacBook-Pro-3:click sidharthshah$ python download_bhavcopy.py --help
Usage: download_bhavcopy.py [OPTIONS] [EXCHANGE]

  download_bhavcopy is utility that will download daily bhav copies from NSE
  and BSE

  Examples: python download_bhavcopy.py bse --for_date 06/12/2017

  python download_bhavcopy.py bse --for_past_days 15

Options:
  --for_date TEXT          Date for which to download bhavcopy DD/MM/YYYY
                           format
  --for_past_days INTEGER  Number of calendar days for which we need to fetch
                           data {E.g. past 15 days from today}
  --help                   Show this message and exit.
```
For reference you can look at [Awesome Screencast on YouTube](https://www.youtube.com/watch?v=kNke39OZ2k0) and [Official Documentation](http://click.pocoo.org/6/commands/)