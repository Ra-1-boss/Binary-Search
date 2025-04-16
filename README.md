# Binary-Search
def binary_search_recursive(
    arr, 
    target, 
    low=None, 
    high=None, 
    return_insert_pos=False
):
    """
    Performs recursive binary search on a sorted array.
    
    Args:
        arr (list): A sorted list of comparable elements (ascending order).
        target: The element to search for.
        low (int, optional): Left boundary of search range. Defaults to 0.
        high (int, optional): Right boundary of search range. Defaults to len(arr)-1.
        return_insert_pos (bool): If True, returns where the target should be inserted if not found.
        
    Returns:
        int: Index of target if found, otherwise -1 (or insertion index if return_insert_pos=True).
    
    Raises:
        ValueError: If array is not sorted in ascending order.
    """
    
    # Initial call validation
    if low is None:
        low = 0
    if high is None:
        high = len(arr) - 1
        
        # Validate that array is sorted (only on first call)
        if any(arr[i] > arr[i+1] for i in range(len(arr)-1)):
            raise ValueError("Array must be sorted in ascending order")
    
    # Base case: target not found
    if low > high:
        return low if return_insert_pos else -1
    
    mid = (low + high) // 2
    
    # Target found
    if arr[mid] == target:
        return mid
    
    # Recursive cases
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, high, return_insert_pos)
    else:
        return binary_search_recursive(arr, target, low, mid - 1, return_insert_pos)


# ====== Enhanced Test Cases ======
def test_binary_search():
    test_cases = [
        ([], 5, (False, 0)),            # Empty array
        ([1, 3, 5, 7], 5, (True, 2)),    # Target present
        ([1, 3, 5, 7], 4, (False, 2)),   # Target missing (insert at 2)
        ([1, 3, 5, 7], 0, (False, 0)),   # Below minimum
        ([1, 3, 5, 7], 8, (False, 4)),   # Above maximum
        ([1, 1, 1, 1], 1, (True, 1)),   # All duplicates
    ]
    
    for arr, target, (should_find, expected_pos) in test_cases:
        try:
            # Test standard search
            result = binary_search_recursive(arr, target)
            assert (result != -1) == should_find
            
            # Test insertion position return
            insert_pos = binary_search_recursive(arr, target, return_insert_pos=True)
            assert insert_pos == expected_pos
            
            print(f"✅ PASS: {arr}, target={target}")
        except AssertionError:
            print(f"❌ FAIL: {arr}, target={target} (got {result}, expected {expected_pos})")


if __name__ == "__main__":
    # Example usage
    data = [2, 4, 6, 8, 10, 12, 14]
    targets = [8, 5, 1, 15]
    
    for target in targets:
        index = binary_search_recursive(data, target)
        if index != -1:
            print(f"Found {target} at index {index}")
        else:
            insert_pos = binary_search_recursive(data, target, return_insert_pos=True)
            print(f"{target} not found. Would be inserted at position {insert_pos}")
    
    # Run comprehensive tests
    print("\nRunning test suite...")
    test_binary_search()
