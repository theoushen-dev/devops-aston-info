#!/bin/bash

URL_TO_CHECK="$1"

if [ -z "$URL_TO_CHECK" ]; then
    echo "Error: URL is not indicated for verification." >&2
    echo "Example: $0 http://app.local" >&2
    exit 1
fi

echo "Check: $URL_TO_CHECK"

if curl -sI --fail --max-time 5 "$URL_TO_CHECK" > /dev/null; then
    echo "Успех: ресурс '$URL_TO_CHECK' is available."
    exit 0
else
    echo "Error: '$URL_TO_CHECK' is not available." >&2
    exit 1
fi