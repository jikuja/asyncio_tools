# asyncio_tools

Useful utilities for working with asyncio - currently in alpha.

## gather

Provides a convenient wrapper around `asyncio.gather`.

```python
from asyncio_tools import gather, CompoundException


async def good():
    return 'OK'


async def bad():
    raise ValueError()


async def main():
    response = await gather(
        good(),
        bad(),
        good()
    )

    # Check if a particular exception was raised.
    ValueError in response.exceptions
    # >>> True

    # To get all exceptions:
    print(response.exceptions)
    # >>> [ValueError()]

    # To get all instances of a particular exception:
    response.exceptions.get_all(ValueError)
    # >>> [ValueError()]

    # To get the number of exceptions:
    print(response.exception_count)
    # >>> 1

    # You can still access all of the results:
    print(response.all)
    # >>> ['OK', ValueError(), 'OK']

    # And can access all successes (i.e. non-exceptions):
    print(response.successes)
    # >>> ['OK', 'OK']

    # To get the number of successes:
    print(response.success_count)
    # >>> 2

    try:
        # To combines all of the exceptions into a single one, which merges the
        # messages.
        raise response.compound_exception
    except CompoundException as compound_exception:
        print("Caught it")

        if ValueError in compound_exception:
            print("Caught a ValueError")

```
