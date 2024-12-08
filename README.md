# Tutorial-Gemini-API
Gemini AI with photos and videos: plate number detection, the output JSON

#Python Code for Plate Number Recognition
import requests
import json
from datetime import datetime

# Replace with your actual API key
API_KEY = 'API_KEY'
API_URL = 'https://api.gemini.ai/v1/vision'

def recognize_plate(image_path):
    # Load the image
    with open(image_path, 'rb') as image_file:
        files = {'file': image_file}
        headers = {'Authorization': f'Bearer {API_KEY}'}
        
        # Send the request to the Gemini API
        response = requests.post(API_URL, headers=headers, files=files)
        
        if response.status_code == 200:
            # Process the response
            data = response.json()
            # Assuming the response contains the required fields
            return {
                'plat_no': data.get('plate_number', 'N/A'),
                'vehicle': data.get('vehicle_type', 'N/A'),
                'vehicle_type': data.get('vehicle_category', 'N/A'),
                'color': data.get('color', 'N/A'),
                'gate_open': datetime.now().strftime('%Y-%m-%d %H.%M.%S'),
                'gate_closed': 'N/A'
            }
        else:
            print("Error:", response.status_code, response.text)
            return None

# Example usage
image_path = 'path_to_your_image.jpg'  # Replace with your image path
result = recognize_plate(image_path)

# Print the result in JSON format
if result:
    print(json.dumps(result, indent=4))

#Output in JSON Format
{
    "plat_no": "B 1234 ABC",
    "vehicle": "car",
    "vehicle_type": "sedan",
    "color": "red",
    "gate_open": "2024-12-02 18.15.01",
    "gate_closed": "N/A"
}
