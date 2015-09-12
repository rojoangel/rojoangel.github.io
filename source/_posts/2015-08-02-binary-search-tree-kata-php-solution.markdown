---
layout: post
title: "Binary Search Tree Kata PHP Solution"
date: 2015-08-02 22:52:17 +0200
comments: true
publish: false
categories: [kata, php]
---
This is my php solution to the Binary Search Tree kata
<!--more-->
{% codeblock lang:php %}

<?php


namespace Kata;


class BinarySearchTree
{

    /** @var int $value */
    private $value;

    /** @var BinarySearchTree $left */
    private $left;

    /** @var BinarySearchTree $right */
    private $right;

    /**
     * @param array $values
     * @return BinarySearchTree
     */
    public static function from($values)
    {
        $values = array_filter(
            $values,
            function ($val) {
                return !is_null($val);
            }
        );

        if (empty($values)) {
            throw new \InvalidArgumentException('values cannot be empty');
        }

        $tree = new BinarySearchTree(array_shift($values));
        foreach ($values as $value) {
            $tree->add($value);
        }

        return $tree;
    }

    /**
     * @param int $value
     */
    private function __construct($value)
    {
        if (is_null($value)) {
            throw new \InvalidArgumentException('value cannot be null');
        }
        $this->value = $value;
    }

    /**
     * @param int $value
     */
    public function add($value)
    {
        if ($value < $this->value) {
            if (empty($this->left)) {
                $this->left = new BinarySearchTree($value);
            } else {
                $this->left->add($value);
            }
        }

        if ($value > $this->value) {
            if (empty($this->right)) {
                $this->right = new BinarySearchTree($value);
            } else {
                $this->right->add($value);
            }
        }
    }

    /**
     * @return array
     */
    public function inOrder()
    {
        $sorted = [];
        if (!empty($this->left)) {
            $sorted = array_merge($sorted, $this->left->inOrder());
        }

        $sorted[] = $this->value;

        if (!empty($this->right)) {
            $sorted = array_merge($sorted, $this->right->inOrder());
        }
        return $sorted;
    }

    /**
     * @return array
     */
    public function preOrder()
    {
        $sorted[] = $this->value;

        if (!empty($this->left)) {
            $sorted = array_merge($sorted, $this->left->preOrder());
        }

        if (!empty($this->right)) {
            $sorted = array_merge($sorted, $this->right->preOrder());
        }
        return $sorted;

    }

    /**
     * @return array
     */
    public function postOrder()
    {
        $sorted = [];

        if (!empty($this->left)) {
            $sorted = array_merge($sorted, $this->left->postOrder());
        }

        if (!empty($this->right)) {
            $sorted = array_merge($sorted, $this->right->postOrder());
        }

        $sorted[] = $this->value;

        return $sorted;
    }
}

{% endcodeblock %}

And this are the tests produced while doing TDD
{% codeblock lang:php %}
<?php


namespace Kata;

use InvalidArgumentException;
use PHPUnit_Framework_TestCase as TestCase;

class BinarySearchTreeTest extends TestCase
{
    /**
     * @param int[] $values
     * @expectedException InvalidArgumentException
     * @expectedExceptionMessage values cannot be empty
     * @dataProvider fromWithEmptyArgumentsDataProvider
     */
    public function testFromWithEmptyArguments($values)
    {
        BinarySearchTree::from($values);
    }

    /**
     * @return array
     */
    public function fromWithEmptyArgumentsDataProvider()
    {
        return [
            'empty values' => [
                []
            ],
            'one null value' => [
                [null]
            ],
            'multiple null values' => [
                [null, null, null, null]
            ]
        ];
    }

    /**
     * @param $values
     * @param $expectedSortedValues
     * @dataProvider providerForTestInOrder
     */
    public function testInOrder($values, $expectedSortedValues)
    {
        $tree = BinarySearchTree::from($values);
        $this->assertEquals($expectedSortedValues, $tree->inOrder());
    }

    /**
     * @return array
     */
    public function providerForTestInOrder()
    {
        return [
            'single positive value' => [
                [4],
                [4]],
            'single zero value' => [
                [0],
                [0]],
            'single negative value' => [
                [-1],
                [-1]],
            'left value #1' => [
                [4, 2],
                [2, 4]],
            'left value #2' => [
                [6, 5],
                [5, 6]],
            'left value #3' => [
                [2, 0],
                [0, 2]],
            'left value #4' => [
                [0, -1],
                [-1, 0]],
            'right value #1' => [
                [2, 4],
                [2, 4]],
            'right value #2' => [
                [5, 6],
                [5, 6]],
            'right value #3' => [
                [0, 2],
                [0, 2]],
            'right value #4' => [
                [-1, 0],
                [-1, 0]],
            'multiple values #1' => [
                [2, 1, 4],
                [1, 2, 4]],
            'multiple values #2' => [
                [1, 2, 4],
                [1, 2, 4]],
            'multiple values #3' => [
                [4, 2, 1],
                [1, 2, 4]],
            'multiple values #4' => [
                [4, 4, 1, null, 1, 2, 2],
                [1, 2, 4]],
            'multiple values #5' => [
                [6, 5, 4, 3, 2, 1],
                [1, 2, 3, 4, 5, 6]],
            'multiple values #6' => [
                [3, 2, 4, 6, 5, 1],
                [1, 2, 3, 4, 5, 6]]
        ];
    }

    /**
     * @param $values
     * @param $expectedSortedValues
     * @dataProvider providerForTestPreOrder
     */
    public function testPreOrder($values, $expectedSortedValues)
    {
        $tree = BinarySearchTree::from($values);
        $this->assertEquals($expectedSortedValues, $tree->preOrder());
    }

    /**
     * @return array
     */
    public function providerForTestPreOrder()
    {
        return [
            'left value #1' => [
                [4, 2],
                [4, 2]],
            'left value #2' => [
                [6, 5],
                [6, 5]],
            'left value #3' => [
                [2, 0],
                [2, 0]],
            'left value #4' => [
                [0, -1],
                [0, -1]],
            'right value #1' => [
                [2, 4],
                [2, 4]],
            'right value #2' => [
                [5, 6],
                [5, 6]],
            'right value #3' => [
                [0, 2],
                [0, 2]],
            'right value #4' => [
                [-1, 0],
                [-1, 0]],
            'multiple values #1' => [
                [2, 1, 4],
                [2, 1, 4]],
            'multiple values #2' => [
                [1, 2, 4],
                [1, 2, 4]],
            'multiple values #3' => [
                [4, 2, 1],
                [4, 2, 1]],
            'multiple values #4' => [
                [4, 4, 1, null, 1, 2, 2],
                [4, 1, 2]],
            'multiple values #5' => [
                [6, 5, 4, 3, 2, 1],
                [6, 5, 4, 3, 2, 1]],
            'multiple values #6' => [
                [3, 2, 4, 6, 5, 1],
                [3, 2, 1, 4, 6, 5]]
        ];
    }

    /**
     * @param $values
     * @param $expectedSortedValues
     * @dataProvider providerForTestPostOrder
     */
    public function testPostOrder($values, $expectedSortedValues)
    {
        $tree = BinarySearchTree::from($values);
        $this->assertEquals($expectedSortedValues, $tree->postOrder());
    }

    /**
     * @return array
     */
    public function providerForTestPostOrder()
    {
        return [
            'left value #1' => [
                [4, 2],
                [2, 4]],
            'left value #2' => [
                [6, 5],
                [5, 6]],
            'left value #3' => [
                [2, 0],
                [0, 2]],
            'left value #4' => [
                [0, -1],
                [-1, 0]],
            'right value #1' => [
                [2, 4],
                [4, 2]],
            'right value #2' => [
                [5, 6],
                [6, 5]],
            'right value #3' => [
                [0, 2],
                [2, 0]],
            'right value #4' => [
                [-1, 0],
                [0, -1]],
            'multiple values #1' => [
                [2, 1, 4],
                [1, 4, 2]],
            'multiple values #2' => [
                [1, 2, 4],
                [4, 2, 1]],
            'multiple values #3' => [
                [4, 2, 1],
                [1, 2, 4]],
            'multiple values #4' => [
                [4, 4, 1, null, 1, 2, 2],
                [2, 1, 4]],
            'multiple values #5' => [
                [6, 5, 4, 3, 2, 1],
                [1, 2, 3, 4, 5, 6]],
            'multiple values #6' => [
                [3, 2, 4, 6, 5, 1],
                [1, 2, 5, 6, 4, 3]]
        ];
    }
}
{% endcodeblock %}
